title: Writing a CSV Todo App with Elixir - part 1/3
date: 2017-09-03 06:35:21
tags:
  - elixir
---

![article cover](http://i.imgur.com/vAzl5xE.jpg)

In this series we will build a simple Todo application that can be used through the Interactive Elixir shell `IEx`. This app will use a `.csv` file to persist data.

We'll have 3 parts, that will cover:

- Part 1: Project setup; reading and parsing files
- Part 2: Creating, updating and deleting todos _(coming soon)_
- Part 3: Displaying todos in a pretty way; filtering todos _(coming soon)_

Some other topics you will see here:

- Elixir's structs
- Module attributes
- Pattern Matching
- Pipe operator
- Unit tests
- Automatic project recompilation

**Obs**: The idea of this project came from the book *Elixir in Action* <a href="#References"><sup>1</sup></a> by Saša Jurić, more precisely in the Chapter 4. The code presented here though is very different of the created in the book and the specifications are also very different so even if you read the book you can still see some interesting stuff here.

## Briefing
Before start coding, let's discuss the basic set of features our app should have to guide our development.

- App should initialize by reading a csv file
- Content from csv (strings/binaries) should be converted into other data structures (structs, maps, etc)
- User should be able to add a new todo and it should be persisted in the csv file
- User should be able to update a todo and it should be persisted in the csv file
- User should be able to delete a todo and it should be deleted in the csv file
- User should be able to see all todos formatted in a readable way
- User should be able to see one specific todo formatted in a readable way
- User should be able to filter todos by `date` and `status`
- Tests should run against a different `.csv` file
- Todos should have the following structure: `id`, `task`, `date`, `status`

We will add some niceties along the way but that's enough to start to think in our API and start to write our tests.

## Project Setup
### Create the project
To create the basic structure of the project, run:

```
mix new todo_list
cd todo_list
```

### Tests
To be able to write our test's files inside the `lib` folder, we need to tweak the `mix.exs` file.

**1)** Go to the file `mix.exs` and add `test_paths: ["lib"]` in the `project` function.

Example:

```elixir mix.exs
def project do
  [
    app: :kv,
    version: "0.1.0",
    elixir: "~> 1.5",
    start_permanent: Mix.env == :prod,
    test_paths: ["lib"],
    deps: deps()
  ]
end
```

**2)** Put the `test/test_helper.exs` file inside the `lib` folder.

**3)** Now you can create all your test's files inside the `lib` folder.

To verify whether everything is working properly, run `mix test`. You can also delete the `test` folder.

### Automatic recompile
When you are testing/using your modules inside `iex`, if you make a change in the code you need to use the function `recompile` to update the current `iex` instance with the latest version of the code or stop and restart the shell.

We will use a tool called `remix` <a href="#References"><sup>2</sup></a> to automate that workflow.

**1)** Add `remix` to your `deps` inside the `mix.exs` file:

```elixir mix.exs
defp deps do
  [
    {:remix, "~> 0.0.1", only: :dev}
  ]
end
```

**2)** Add `:remix` as a development only OTP app.

```elixir mix.exs
def application do
  [
    applications: applications(Mix.env),
    extra_applications: [:logger]
  ]
end

defp applications(:dev), do: applications(:all) ++ [:remix]
defp applications(_all), do: [:logger]
```

**3)** Install `remix` typing `mix deps.get`

## CSV files
This type of file works as a spreadsheet. Each line of the file is like a row, and each "cell" is separated by a comma.

Create two files inside the `lib` folder:

```sh
touch lib/{todos,todos_test}.csv
```

Let's add some data in both:

```sh
echo "1,Study Elixir,2017-10-01,done" > lib/todos.csv &&
echo "1,Study Erlang,2018-01-01,todo" > lib/todos_test.csv
```

## Reading the CSV files
We need an `init` function to bootstrap our app and read the content inside the `.csv` file.

Let's write an assertion in our test file before implement the function.

```elixir lib/todo_list_test.exs
defmodule TodoListTest do
  use ExUnit.Case
  doctest TodoList

  test "if the app will load the data from the csv file correctly" do
    assert TodoList.init == "1,Study Erlang,2018-01-01,todo"
  end
end
```

To verify everything is working fine, add the following code in your `lib/todo_list.ex` file:

```elixir lib/todo_list.ex
defmodule TodoList do
  @moduledoc """
  Todo list application to work with .csv files through IEx.
  """

  def init do
    "1,Study Erlang,2018-01-01,todo"
  end
end
```

Running `mix test` you should see this:

```
> mix test
.

Finished in 0.02 seconds
1 test, 0 failures

Randomized with seed 941386
```

We are good to start ~~breaking~~ making some cool stuff!

### Module attributes and Environment variables
To decide which file our code will use to read/write information, we will use a combination between Elixir's module attributes and Mix's environment variables (atoms).

When we run our tests using `mix test`, `mix` sets its `env` value to `:test` (it's an atom). The same occur when we run our code with `iex -S mix`, but the actual value we get with `Mix.env` is `:dev`. In that way we can dynamically opt to use `todos.csv` or `todos_test.csv`.

We can define some module attributes in Elixir, that will work like a constant inside such module.

Let's use these features to decide where to read the content, improving our `init` function.

### Building the `Path`
First of all, we will need a module attribute that holds a map so we can associate each key in this map with the `:dev` and `:test` atoms. Add the following code below the `TodoList` module definition.

```elixir lib/todo_list.ex
defmodule TodoList do

  @path_env %{dev: "todos.csv", test: "todos_test.csv"}
```

Instead of keep our value as a string, e.g (`"lib/todos.csv"`), we will use the module `Path` to generate the path for us. In that way, the `join` function can handle OS' specificities for us while building the path.

Add another module attribute to generate the final path:

```elixir lib/todo_list.ex
defmodule TodoList do

  @path_env %{dev: "todos.csv", test: "todos_test.csv"}
  @path Path.join(["lib", @path_env[Mix.env]])
```

### `File.stream!`
In order to keep our functions as simple, specialized and reusable as possible, we will create a helper function to actually read the contents inside the `.csv` files. Because it is a helper function, we will define it as a private function with the `defp` construct. Your code will look like this:

```elixir lib/todo_list.ex
defmodule TodoList do
  @moduledoc """
  Todo list application to work with .csv files through IEx.
  """

  @path_env %{dev: ["lib", "todos.csv"], test: ["lib", "todos_test.csv"]}
  @path Path.join(@path_env[Mix.env])

  def init do
    @path
    |> read_file!
  end

  defp read_file!(path) do
    path
    |> File.stream!
    |> Enum.map(&String.replace(&1, "\n", ""))
    |> to_string
  end
end
```

In the `init` function we are piping the `@path` to the `read_file!` function. There, we are using the `File.stream!` function to actually read the file, removing all *newline* characters (`\n`) and converting the content `to_string` to match the expected value in our tests.

If you run `mix test` now, your test should still be passing :)

> Check the commit here: [b03097e](https://github.com/ericdouglas/ex_todo_csv/commit/b03097e05af392eb2e067b27251092c6b256597b)

### Data structures
Although we can keep moving forward with our todo app using strings/binaries, Elixir has other useful data structures that will facilitate our work if we convert the initial content retrieved from the `.csv` file to such types.

Let's see a diagram of the content we will deal with:

![todo list with two todos](http://i.imgur.com/P6XKk0J.jpg)

We have a todo list that contains several todos and a reference for the last id created, in order to create the next todo with `last_id + 1` in our case.

The `todo` contains 4 properties that we already now from the specs.

With such diagram in mind, we can create a `struct` for the todo list and todos to enforce the proper usage of such elements.

### Defining Structs
We can have only one struct per module, so let's first define our `TodoList` struct:

```elixir lib/todo_list.ex
defmodule TodoList do
  defstruct last_id: 0, todos: %{}
```

Now let's create another file called `todo.ex` to define the `Todo` struct:

```elixir lib/todo.ex
defmodule Todo do
  defstruct [:id, :task, :date, :status]
end
```

When all values you will define in the struct are `nil`, you can pass a *keyword list* for the `defstruct` function. It's the equivalent of:

```elixir
  defstruct id: nil, task: nil, date: nil, status: nil
  # equivalent of defstruct [:id, :task, :date, :status]
```

### Data transformation
Now we have the structs defined, let's refactor our program to read the `.csv` file but returns a `%TodoList{}` struct with the content inside. Before it though, let's refactor ours tests to assert we are parsing the file and transforming its data to the expected structures:

```elixir lib/todo_list_test.ex
defmodule TodoListTest do
  use ExUnit.Case
  doctest TodoList

  test "if the app will load and transform the data from the csv file correctly" do
    assert TodoList.init == %TodoList{
      last_id: 1,
      todos: %{
        1 => %Todo{
          id: 1,
          task: "Study Erlang",
          date: "2018-01-01",
          status: "todo"
        }
      }
    }
  end
end
```

Pay attention to the structure that we will convert the binaries we read to. Much more pleasant to work for sure.

Let's refactor our program now to meet this expectation:

```elixir lib/todo_list.ex
defmodule TodoList do
  @moduledoc """
  Todo list application to work with .csv files through IEx.
  """
  defstruct last_id: 0, todos: %{}

  @path_env %{dev: ["lib", "todos.csv"], test: ["lib", "todos_test.csv"]}
  @path Path.join(@path_env[Mix.env])

  def init do
    @path
    |> read_file!
    |> format_to_work
  end

  defp read_file!(path) do
    path
    |> File.stream!
    |> Stream.map(&String.replace(&1, "\n", ""))
  end

  defp format_to_work(input) do
    format_todos = fn(el, acc) ->
      [id, task, date, status] = String.split(el, ",")
      id = String.to_integer(id)

      Map.put(acc, id, %Todo{id: id, task: task, date: date, status: status})
    end

    todos   = Enum.reduce(input, %{}, format_todos)
    last_id = Map.keys(todos) |> Enum.max

    %TodoList{last_id: last_id, todos: todos}
  end
end
```

> Check the commit here: [c39db44](https://github.com/ericdouglas/ex_todo_csv/commit/c39db44b5c9c7da59bfbb88614fcc7a9fd8855c4)

In our `read_file/1` function we now keep the data in its `stream` form so it will be lazy evaluated.

In our `init/0` function we added another function in the pipe, `format_to_work/1`. Its responsibility is to break the comma separated values and put each of those values inside the right `struct`ure.

Look in the line 30 when we pass a lambda (anonymous function) to the `reduce` function :)

Run your test to verify it is still passing. **Awesome**!

## Interacting with the program using `iex`
Now we have our first feature implemented, let's interact with our program. Type the following commands in your command line:

```
> iex -S mix

iex(1)> TodoList.init

%TodoList{last_id: 1,
 todos: %{1 => %Todo{date: "2017-10-01", id: 1, status: "done",
    task: "Study Elixir"}}}
```

You enter in `iex`, then `init` the program, and see the content of the `.csv` file parsed as the return of the `init` function.

## Writing `doctests`
A very useful feature Elixir has built-in is the possibility to write tests inside our documentation that will run together with our other tests.

Let's write a documentation for the `init/0` function with its respective test.

```elixir lib/todo_list.ex
@doc """
Read the .csv file and return its content formatted

## Examples

    iex> TodoList.init
    %TodoList{
      last_id: 1,
      todos: %{
        1 => %Todo{
          id: 1,
          task: "Study Erlang",
          date: "2018-01-01",
          status: "todo"
        }
      }
    }
"""
def init do
  @path
  |> read_file!
  |> format_to_work
end
```

> Check the commit here: [98af109](https://github.com/ericdouglas/ex_todo_csv/commit/98af109bbdbc21c7bd4559a1870eac267917e104)

Run your tests again and you will see `2 tests` in the message :)

```
> mix test
..

Finished in 0.04 seconds
2 tests, 0 failures
```

## Summary
This concludes our first article in the series about how to create a basic csv todo application with Elixir.

We saw in this article:

- How to create a new Elixir project
- How to run our tests and put them alongside the code they will actually test
- How to recompile our project automatically
- How to read a file
- How to use some Elixir's data structures
- How to write doctests

And that's it!

> See you in the next article. Cheers!

 ## References
1. [Elixir in Action](https://www.manning.com/books/elixir-in-action)
1. [remix](https://github.com/AgilionApps/remix)

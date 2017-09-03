title: Writing a CSV Todo App with Elixir
date: 2017-09-03 06:35:21
tags:
  - elixir
---

In this article we will build a simple Todo application that can be used through the Interactive Elixir shell `IEx`. This app will use a `.csv` file to persist the data.

Some interesting topics you will see here:

- Elixir's structs
- Module attributes
- Pattern Matching
- Pipe operator
- Write and read *Comma Separated Values* files (`.csv`)
- Data transformation
- Unit tests
- Project configuration based on environment variables
- Automatic project recompilation
- Print formatted text to terminal (bold, italic, etc)

**Obs**: The idea of this project came from the book Elixir in Action by Saša Jurić, more precisely in the Chapter 4. The code presented here though is very different of the created in the book and the specifications are also very different so even if you read the book you can still see some interesting stuff here.

## Briefing
Before start coding, let's discuss the basic set of features our app should has to guide our development.

- App should initialize reading a csv file
- Content from csv (strings/binaries) should be converted into other data structures (structs, maps, etc)
- User should be able to add a new todo and it should be persisted in the csv file
- User should be able to update a todo and it should be persisted in the csv file
- User should be able to delete a new todo and it should be delete in the csv file
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
mix new ex_todo_csv
cd ex_todo_csv
```

### Tests
To be able to write our tests files inside the `lib` folder, we need to tweak the `mix.exs` file.

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

**3)** Now you can create all your test files inside the `lib` folder.

To verify everything is working, run `mix test`. You can also delete the `test` folder.

### Automatic recompile
When you are testing your modules inside `iex`, if you make a change in the code you need to use the function `recompile` to update the current `iex` instance with the latest version of the code or stop and restart the shell.

We will use a tool called `remix` to automate that workflow.

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

## References
- [Elixir in Action](https://www.manning.com/books/elixir-in-action)
- [remix](https://github.com/AgilionApps/remix)
- [How to format text in terminal](https://askubuntu.com/questions/528928/how-to-do-underline-bold-italic-strikethrough-color-background-and-size-i)
- []()
- []()
- []()

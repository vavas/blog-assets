title: Practicing Elixir - or any - programming language
date: 2017-08-24 14:31:28
tags:
  - elixir
  - testing
  - tdd
  - algorithms
---

![algorithm in elixir](http://i.imgur.com/LsHtF9t.png)
> Image generated on [Instacode](http://instaco.de/113274) - [algorithm](https://github.com/ericdouglas/labs/blob/master/hackerrank/algorithms/old-elixir/005-diagonal-difference/005_diagonal_difference.ex)

In this article I'll show how I'm learning/practicing Elixir, but the steps showed here aim to be valid for any new language one want to learn/practice.

The steps are basically:

1. Understand the pros and cons of the language
1. Familiarize with the syntax and semantic of the language
1. Learn some|enough constructs to start expressing yourself in the language
1. Practice the acquired knowledge resolving some algorithm challenges
1. Return to step 3

I read the [Elixir guides](https://elixir-lang.org/getting-started/introduction.html) in chunks and started to practice alongside on [HackerRank](https://www.hackerrank.com/domains/algorithms/warmup) to really internalize what I was learning.

I'm liking this approach very much so here are some tips that are not so obvious on how you can have some fun while learning a new programming language.

## Why algorithms?
Because this kind of problem is an interesting way to put yourself searching all the API of the language in order to solve the challenge in the more elegant/concise way you currently can.

Since some challenges may not be so common, you can end up learning new tricks that will help you on your daily routine as well.

## Write tests
Test-driven development is a [polemic topic](https://elixirforum.com/t/bdd-tdd-criticized/759) but this scenario (algorithms) is one that you can have real benefits of such approach.

It will help you test your algorithm much faster and precisely, with almost no effort. You just need to run one command.

In the end of the article I'll solve an algorithm with you so we'll see how to handle this task.

## Backup your algorithms somewhere
Don't keep your solutions only in the site you are using to test your knowledge. One reason to develop everything on your machine is to start from the day zero to have a real experience with all the working environment you will inevitable face if you start to create more ambitious programs in that new language.

[GitHub | BitBucket | GitLab] are great places to store your work so you can also easily access it if needed.

Personally, I always search code I put there to remember some aspect of the tool I'm currently using that I forgot. **It's very helpful!**

## Blocks of theory and practice
Another useful tip is: put some milestones in your study so after read X chapters of a book or Y pages on the documentation, you will solve N exercises.

Example:

- After each chapter of `<Book>` that I read, I'll solve 3 exercises on `<code challenge site>`

It's also useful take some notes while reading your resources, so you can read it after you finish the exercises. The flux will be:

1. Theory
1. Exercises
1. Review annotations

## Let's code!
Now we'll solve the last exercise in the warmup section of HackerRank.

I chose this exercise - [Time Conversion](https://www.hackerrank.com/challenges/time-conversion) - because looking the statistics, it's the hardest in that section but using Elixir to solve this end up being really easy.

Since we are using a TDD approach, let's first write our tests.

> **[You can check the final source code here](https://github.com/ericdouglas/labs/tree/master/hackerrank/algorithms/old-elixir/010-time-conversion)**

## Test Setup
Elixir has a built-in test tool called [ExUnit](https://hexdocs.pm/ex_unit/ExUnit.html), so our work here is pretty minimal.

Let's create a file called `time_conversion_test.exs`.

After this, put the following code in such file to have the basic setup in place:

```elixir
ExUnit.start()

defmodule TimeConversionTest do
  use ExUnit.Case

  test "the truth" do
    assert 1 == 1
  end
end
```

It's done! :)

Go to your terminal and type `elixir time_conversion_test.exs`. You will se the following:

```
> elixir time_conversion_test.exs

.

Finished in 0.08 seconds (0.07s on load, 0.01s on tests)
1 test, 0 failures

Randomized with seed 501932
```

Now you verified the test tool is working, let's write all tests we need for this algorithm.

You can already import the module we will actually test, so add `import TimeConversion` above the `test` case.

> **Important:** since you are importing a module from another file, you will need to compile such file every time before you run your tests in order to get the last version of it. We'll see how to use a tool to automate such task below. For now, to compile your code you need to run `elixirc name_of_file.ex`. Only the module/file where the algorithm is created needs to be compiled. You don't need to compile test files!

We will test every *hour* and *hour and half* to assure our program works in all cases. Your final test file should looks like this:

> The `main` function you will see is actually the `main` function from the `TimeConversion` module we will write next. Since we imported it, we can use the `main` function directly.
>
> If you prefer, you can use `TimeConversion.main()` without import `TimeConversion` as well.

```elixir
ExUnit.start()

defmodule TimeConversionTest do
  use ExUnit.Case
  import TimeConversion

  test "Convert 12-hour time to 24-hour format" do
    assert main("12:00:00AM") == "00:00:00"
    assert main("12:33:11AM") == "00:33:11"

    assert main("01:00:00AM") == "01:00:00"
    assert main("01:33:11AM") == "01:33:11"

    assert main("02:00:00AM") == "02:00:00"
    assert main("02:33:11AM") == "02:33:11"

    assert main("03:00:00AM") == "03:00:00"
    assert main("03:33:11AM") == "03:33:11"

    assert main("04:00:00AM") == "04:00:00"
    assert main("04:33:11AM") == "04:33:11"

    assert main("05:00:00AM") == "05:00:00"
    assert main("05:33:11AM") == "05:33:11"

    assert main("06:00:00AM") == "06:00:00"
    assert main("06:33:11AM") == "06:33:11"

    assert main("07:00:00AM") == "07:00:00"
    assert main("07:33:11AM") == "07:33:11"

    assert main("08:00:00AM") == "08:00:00"
    assert main("08:33:11AM") == "08:33:11"

    assert main("09:00:00AM") == "09:00:00"
    assert main("09:33:11AM") == "09:33:11"

    assert main("10:00:00AM") == "10:00:00"
    assert main("10:33:11AM") == "10:33:11"

    assert main("11:00:00AM") == "11:00:00"
    assert main("11:33:11AM") == "11:33:11"

    assert main("12:00:00PM") == "12:00:00"
    assert main("12:33:11PM") == "12:33:11"

    assert main("01:00:00PM") == "13:00:00"
    assert main("01:33:11PM") == "13:33:11"

    assert main("02:00:00PM") == "14:00:00"
    assert main("02:33:11PM") == "14:33:11"

    assert main("03:00:00PM") == "15:00:00"
    assert main("03:33:11PM") == "15:33:11"

    assert main("04:00:00PM") == "16:00:00"
    assert main("04:33:11PM") == "16:33:11"

    assert main("05:00:00PM") == "17:00:00"
    assert main("05:33:11PM") == "17:33:11"

    assert main("06:00:00PM") == "18:00:00"
    assert main("06:33:11PM") == "18:33:11"

    assert main("07:00:00PM") == "19:00:00"
    assert main("07:33:11PM") == "19:33:11"

    assert main("08:00:00PM") == "20:00:00"
    assert main("08:33:11PM") == "20:33:11"

    assert main("09:00:00PM") == "21:00:00"
    assert main("09:33:11PM") == "21:33:11"

    assert main("10:00:00PM") == "22:00:00"
    assert main("10:33:11PM") == "22:33:11"

    assert main("11:00:00PM") == "23:00:00"
    assert main("11:33:11PM") == "23:33:11"
  end
end
```

Now let's create our algorithm!

## I/O Strategy
Since HackerRank pass the input for our algorithm using the stdin (standard input stream) and read our solution from the stdout (standard output stream), a great tip here is to **put that side-effect part outside of your main module**. This will assure you are working in a more functional style/mindset.

The mental model here is:

1. Create a module that will be your solution for the challenge
1. Pass the necessary input to solve the challenge for the `main` function in such module
1. Create helper functions inside your module and pipe then in the `main` function
1. Return the answer from the `main` function and pipe it to the stdout

Let's create our file `time_conversion.ex` and then configure the basic setup for such approach:

```elixir
defmodule TimeConversion do
  def main(input) do
    input
  end
end

input = IO.gets("") |> String.trim
TimeConversion.main(input) |> IO.puts
```

As you can see, we are passing the input in the end of the file to our `main` function and piping it to the `IO.puts` function that will print the result to the console.

Now the last thing you need to do is actually figure out how to solve the problem! :D

> I recommend you to try solve this before see the solution. It's a very interesting challenge!

## Solving the challenge
Personally, I find the final solution for such challenge quite elegant using the pipe operator `|>` and the pattern matching Elixir give to us.

One interesting point to note as well is that the `+` operator in Elixir is actually a function so I could use it in the pipeline as `Kernel.+()`. Really cool!

The final solution:

> **Update:** after [these](https://news.ycombinator.com/item?id=15096942) great suggestions, I refactored the code for the current version you'll se below. You can see the old version [here](https://github.com/ericdouglas/labs/commit/670a3fc7ddea5f5bf5dab9ce8a5c717be28f15bf).
>
> In the current version I removed `Kernel.+()` and used string interpolation `#{}` instead.

```elixir
defmodule TimeConversion do
  def main(input) do
    input |> converter
  end

  def converter(input) do
    [hours, minutes, seconds] = String.split(input, ":")
    period                    = String.slice(seconds, 2..3)
    seconds                   = String.slice(seconds, 0..1)
    formated_hour             = format_hour(period, hours)

    "#{formated_hour}:#{minutes}:#{seconds}"
  end

  defp format_hour("AM", "12"), do: "00"
  defp format_hour("AM", hours), do: hours
  defp format_hour("PM", "12"), do: "12"
  defp format_hour("PM", hours), do: "#{String.to_integer(hours) + 12}"
end

input = IO.gets("") |> String.trim
TimeConversion.main(input) |> IO.puts
```

## Why the `mix` tool?
Following the philosophy to use a real environment while practicing a new language, the method showed now is more closely to what you will see in the wild :)

`mix` is a tool to manage your Elixir applications. It can handle project's dependencies, run your tests, compile your code, and so on.

The great benefit we will have using `mix` is that it compile our code before run the tests so we don't need to bother with such task everytime we want to test our algorithm. Quite handy! The dowside is you need to comment the side-effect part of the code in order to achieve such benefit, not a big deal. You can uncomment it when you paste your code on HackerRank.

## Using `mix`
To create a new project, type:

```
mix new project_name
```

In our case, let's use `mix new elixir_algorithms`.

Now you have a basic structure to work upon. We will change where mix run the tests so our test files can stay alongside the actual code they are testing.

## Configuring your test setup - again :)
**1)** Go to the file `mix.exs` and add `test_paths: ["lib"]` in the `project` function.

Example:
```elixir
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

**2)** Put the `test_helper.exs` file inside the `lib` folder.

**3)** Now you can create all your test files inside the `lib` folder.

To verify everything is working, run `mix test`.

Check our project with the new structure **[here](https://github.com/ericdouglas/labs/tree/master/hackerrank/algorithms/elixir_algorithms/lib)**.

> That's it. **Happy coding!**

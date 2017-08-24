title: Practicing the Elixir language (or any new programming language)
date: 2017-08-24 14:31:28
tags:
---

![algorithm in elixir](http://i.imgur.com/LsHtF9t.png)
> Image generated on [Instacode](http://instaco.de/113274)

In this article I'll show how I'm learning/practicing Elixir, but the steps showed here aim to be valid for any new language one want to learn/practice.

The steps are basically:

1. Understand the pros and cons of the language
1. Familiarize with the syntax and semantic of the language
1. Learn some|enough constructs to start expressing yourself in the language
1. Practice the acquired knowledge resolving some algorithm challenges
1. Return to step 3

I read the [Elixir guides](https://elixir-lang.org/getting-started/introduction.html) in chunks and started to practice alongside on [HackerRank](https://www.hackerrank.com/domains/algorithms/warmup) to really internalize what I was learning.

I'm liking this approach very much so here are some more tips on how you can have some fun while learning a new programming language that are not so obvious.

## Why algorithms?
Because this kind of problem is an interesting way to put yourself searching all the API of the language in order to solve the challenge in the more elegant/concise way.

Since some challenges may be not so common, you can end up learning new tricks that will help you on your daily routine as well.

## Write tests
Test-driven development is a [polemic topic](https://elixirforum.com/t/bdd-tdd-criticized/759) but this scenario (algorithms) is one that you can really benefit of such approach.

It will help you test your algorithm much faster and precisely, with almost no effort, just run one command.

In the end of the article I'll solve an algorithm with you so we'll see how to handle this task.

> Pay attention there since it will have precious tips! :)

## Backup your algorithms somewhere
Don't keep you solutions only in the site you are using to test your knowledge. One reason to develop everything on your machine is to start from the day zero to have a real experience with all the working environment you will inevitable face if you start to create more ambitious programs in the such new language.

[GitHub | BitBucket | GitLab] are great places to store your work so you can also easily access it if needed. Personally, I always search code I put there to remember some aspect of the tool I'm currently using that I forgot. **It's really useful and very helpful**.

## X exercises per step
Another great tip is: put some milestones in your study so after read X chapters of a book, Y pages on the documentation, you will solve Z exercises.

Example:

- After I read 1 chapter of `<Elixir Book>`, I'll solve 3 exercises on `<code challenge site>`

## Let's code!
Now I'll solve the last exercise in the warmup section of HackerRank with you.

I chose this exercise - [Time Conversion](https://www.hackerrank.com/challenges/time-conversion) because looking the statistics, it's the most hard in that section but using Elixir to solve this end up being really easy.

Since HackerRank pass the input for your algorithm using the stdin (standard input stream) and read your solution from the stdout (standard output stream), a great tip here is to put that side-effect part outside of your main module.

But since we are using a TDD approach, let's first write our tests.

> **[Check the source code here](https://github.com/ericdouglas/labs/tree/master/hackerrank/algorithms/elixir/010-time-conversion)**

## Test Setup
Elixir has a built-in test tool called [ExUnit](https://hexdocs.pm/ex_unit/ExUnit.html), so our work here is pretty minimal.

Let's create a file called `time_conversion_test.exs`. After this, the basic setup we need is:

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

# Conditionals

Now that we can get input from our users, we can start to make our programs
smart, by behaving differently based on different conditions.

To do this, we need to add a new grammar to our toolbox: the `if`/`end` **keywords**. Here's how it looks:

<iframe frameborder="0" width="100%" height="600px" src="https://repl.it/@raghubetina/first-conditional?lite=true"></iframe>

Try running this program a few times and see how it behaves.

## The basic anatomy of if statements

The anatomy of an `if` statement is:

```ruby
if condition
  # code that runs if the condition is true
end
```

 1. First comes the `if` keyword.
 1. After the `if`, on the same line, comes any Ruby expression, which is evaluated until only one piece of data is left.
 1. If that final return value of `condition` is "truthy", then the code on the lines between the `if` and `end` keywords is executed.
 1. If the final return value of `condition` is "falsy", then the code on the lines between the `if` and `end` keywords is ignored.
 1. Either way, the program picks up execution on the next line after the `end` keyword and continues on.

## truthiness and falsiness

Why did I say "truthy" and "falsy" instead of just `true` and `false`? Because many — most — Ruby expressions return values other than `true` or `false`. _Any_ object can appear next to an `if`, and some will cause the code inside the `if` statement to execute (these values are known as "truthy") and some will not (these are "falsy").

In the REPL below, try replacing `lucky_number.odd?` with each of the following. Before clicking "run" for each one, ask yourself, do you expect to see the output `"The condition is truthy."` or not?

 - `0`
 - `"false"`
 - `[]`
 - `nil`
 - `true`
 - `""`
 - `false`

<iframe frameborder="0" width="100%" height="600px" src="https://repl.it/@raghubetina/truthiness?lite=true"></iframe>

For how many of the above did you correctly predict the output? What did you learn about what objects count as truthy and what objects count as falsy in Ruby?

It turns out that **only `false` and `nil` are falsy**. _All_ other objects in Ruby are truthy — even `0` and `""`.

## Comparisons

That said, we'll mostly use expressions after `if` that return `true` or `false`. There are lot of methods that are designed to do this; we've seen `Integer`'s `.odd?` and `.even?`, but there are a lot more.

For example, classes have ways to compare _instances_ of the class to one another:

```ruby
1 < 2          # "1 is less than 2"
2 < 1          # "2 is less than 1"
24*365 > 10000 # There are more than 10,000 hours in a year
1 == 1         # "1 is equivalent to 1"
1 == 2         # "1 is equivalent to 2"
1 != 1         # "1 is NOT equivalent to 1"
1 != 2         # "1 is NOT equivalent to 2"
"apple" < "banana"
"apple" > "banana"
"apple" == "banana"
"apple" != "banana"
```

Note the difference between the **equivalence operator** — two equals signs, `==` — and the variable assignment operator — one equals sign, `=`. Mixing up the two of them is probably _the_ most common typo programmers make. Don't say I didn't warn you!

## Multibranch if statements

We can also have **multibranch** `if` statements, where we specify fallback conditions to check and code to execute if the first condition is falsy:

<iframe frameborder="0" width="100%" height="600px" src="https://repl.it/@raghubetina/multibranch-if?lite=true"></iframe>

 - Note that **there is no space** in the `elsif` keyword.
 - The conditions are checked in top-down priority, so even if more than one is true, whichever one is first has its branch executed; the rest are ignored.
 - If none are true, the final `else` fallback branch is executed; but you don't have to have one if you don't want one.

Inside a branch of an `if` statement, you can have as many lines of code as you want — and you can even have whole other multi-branch if statements, if that's what you need.

## Combining conditions with AND and OR

Finally, another handy thing to have in your toolbelt are the **logical operators** `&&` and `||`. These allow you to combine comparisons; try these out below:

```ruby
1 < 2 && 2 < 3   # Is 1 less than 2 AND 2 less than 3?
1 < 2 && 3 < 2   # Is 1 less than 2 AND 3 less than 2?
2 < 1 && 3 < 2   # Is 2 less than 1 AND 3 less than 2?
1 < 2 || 2 < 3   # Is 1 less than 2 OR 2 less than 3?
1 < 2 || 3 < 2   # Is 1 less than 2 OR 3 less than 2?
2 < 1 || 3 < 2   # Is 2 less than 1 OR 3 less than 2?
```

<iframe frameborder="0" width="100%" height="600px" src="https://repl.it/@raghubetina/and-and-or?lite=true"></iframe>

Basically, `&&` is stricter than `||`; both comparisons have to be true in order for the whole statement to be true when combined with `&&`; either one being true is sufficient for `||`.

## Challenge

Can you create a simple Rock, Paper, Scissors game?

<iframe frameborder="0" width="100%" height="600px" src="https://repl.it/student_embed/assignment/3079909/05408ed63586e04010be2c0788354f60"></iframe>

Now, can you create a better version of the game, where the computer chooses its move randomly instead of always choosing scissors?

<iframe frameborder="0" width="100%" height="600px" src="https://repl.it/@raghubetina/rps-2?lite=true"></iframe>

# Handling exceptions with begin/rescue/end

In Ruby, an unwanted or unexpected event results in an _exception_, which in the normal flow of things results in our program terminating with an error message. Usually, this is a good thing; we should **R**ead **T**he **E**rror **M**essage, learn from it, and improve our code to make it error-proof.

But in some rare cases, our programs need to be able to tolerate exceptions and not have them interrupt the execution of our program. For example, if we're scraping data from potentially unreliable websites, out of a thousand different links we're clicking, one of them could be inaccessible; we don't want this one link to crash our program.

For these cases, Ruby provides the `begin`/`rescue`/`end` construct. You can think of it sort of like an `if`/`else`/`end` that prevents code from error-ing out:

<iframe frameborder="0" width="100%" height="600px" src="https://repl.it/@raghubetina/01-simple-rescue?lite=true"></iframe>

Notice that this program completes successfully, even though `7.capitalize` _should_ crash with an `undefined method` exception! Try changing the code in the `begin` branch to `7.odd?` and see how the output changes.

## More details about the exception

It turns out that, like everything else in Ruby, exceptions are objects — descendants of class `Exception`. We can get access to the exception that occurred and get information about it by making up a variable name after the `rescue` keyword, like this:

<iframe frameborder="0" width="100%" height="600px" src="https://repl.it/@raghubetina/02-exception-message?lite=true"></iframe>

Most commonly, the method we use on the exception is `.message`, which returns a string that we print or log somehow.

## Further reading

Using `begin`/`rescue`/`end` should be pretty rare. Most of the time, if you have an exception in your code, you should debug it. But for the rare times that you are legitimately interfacing with an unreliable external system, it's a handy tool in your belt.

The above usage of `begin`/`rescue`/`end` will get the job done 95% of the time, but [here is some further reading on a lot more ways of using it](https://stackify.com/rescue-exceptions-ruby/){:target="_blank"}.



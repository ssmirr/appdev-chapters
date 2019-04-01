# A few program notes

Before we continue, I want to pause and discuss what, why, and how we're going to be learning.

## What's our goal?

Our goal is to build and deploy a fully-functional application, which will require that we learn a little bit about every part of the "stack" — the interface that users interact with all the way through to the server that the application runs on.

We're going to work from the inside-out. We'll start by learning how to manage information with a _database_, because that is the heart of any useful application. Then we're going to learn how to expose an _interface_ for users to get information in and out of our database — we'll be starting with a web client because that's the quickest way to get up and running, but we could add an iPhone or Android client later.

## Why Ruby?

For all of the above, we're going to use the Ruby language. There are many languages we could have chosen, but Ruby has a lot of advantages for beginners:

### Developer happiness

Compared to other languages, Ruby is a pleasure to write and to read. Here's Yukihiro Matsumoto (a.k.a. "Matz"), the creator of Ruby:

> Often people, especially computer engineers, focus on the machines. They think, "By doing this, the machine will run faster. By doing this, the machine will run more effectively. By doing this, the machine will something something something." They are focusing on machines. But in fact we need to focus on humans, on how humans care about doing programming or operating the application of the machines.
>
> — Yukihiro Matsumoto, [The Philosophy of Ruby](https://www.artima.com/intv/ruby4.html){:target="_blank"}

Matz's focus when he designed Ruby was on "developer happiness", which was pretty bold in back 1995 when Ruby was first released. Optimizing for human readability rather than computer readability meant paying a cost in terms of performance, and computers were slow back then; but Matz didn't care.

Happily, computers are very fast today[^developer_hours], and they can run Ruby just fine; so now we beginners can have the best of both worlds. Besides, once you've learned the basic concepts of programming, they're not hard to translate into the syntax of another language[^right_tool].

[^right_tool]:
    If you decide to pursue software development, you'll end up learning at _least_ half-a-dozen languages. Whatever challenge is thrown your way, you'll choose the right tool for the job.

[^developer_hours]: From a business perspective, servers are very cheap while developers are _very_ expensive; so you should pick the language that makes developers the most productive.

### Ruby on Rails and other libraries

People often ask "What's the best language for task X?" Depending on the task, sometimes a language is particularly well-suited to it technically; but that is very rare. In general, any programming language can do anything that any other language can (unless maybe there is some kind of proprietary platform lock-out).

However, there's another very important consideration: which language has the largest **community** of developers doing task X? More community means more shared code (known as "libraries", or in Ruby-land, "gems"), more blog posts, more answers when you Google a question, etc.

For example, Python and Ruby are very similar languages in terms of their technical features and performance profile. However, by some historical accident, Python seems to have gathered more of the scientific/data analysis/machine learning community around it, so more open-source libraries in those areas have been developed and shared in Python than in Ruby; and so now Python is the go-to language for those tasks.

On the other hand, for agile development of database-backed applications, Ruby has a huge and thriving community. In particular, there is an open-source library for building applications called Ruby on Rails ("Rails", for short) that makes small teams or even solo developers incredibly productive. The existence of Rails alone makes the Ruby language a good choice[^rails_relevant] for both startups and beginners.

[^rails_relevant]:
    Here's a longer blog post on why Ruby on Rails is still a good choice in 2019:

    [https://devbrett.com/2019/03/why-i-believe-rails-is-still-relevant-in-2019.html](https://devbrett.com/2019/03/why-i-believe-rails-is-still-relevant-in-2019.html){:target="_blank"}

The philosophy of Rails is "convention over configuration" — it makes a lot of decisions on your behalf, and if you go with the flow, then things "just work". (If you want to fiddle with settings, then of course you can, to your heart's content; but you don't _have to_ spend hours or days doing so before anything will even show up, like you do in most other frameworks.) You can focus on building the unique features of _your_ application, not plumbing.

There are a lot of other powerful, easy-to-use Ruby libraries that have philosophies similar to Rails. Ruby seems to have gathered a community of developers around it who are more about creating value for our users, and less about [bike shedding](https://en.wiktionary.org/wiki/bikeshedding){:target="_blank"} over technical details[^choose_boring].

[^choose_boring]:
    I, personally, agree with this author who prefers boring technologies over cutting-edge ones:

    [https://mcfunley.com/choose-boring-technology](https://mcfunley.com/choose-boring-technology){:target="_blank"}

### MINASWAN

The programming world at large can be pretty competitive and harsh, but I've found that the Ruby community is very inclusive and welcoming, which is a relief for beginners. Maybe this can also be traced back to Matz; from [his Wikipedia page](https://en.wikipedia.org/wiki/Yukihiro_Matsumoto){:target="_blank"},

> His demeanor has brought about a motto in the Ruby community: "Matz is nice and so we are nice," commonly abbreviated as MINASWAN.

### Heroku and other integrations

Another way in which the Ruby community's philosophy seems to have manifested is the amazing amount of tooling that exists around it. For example, [Heroku](https://heroku.com) was the first "platform as a service", a layer on top of Amazon Web Services that makes it incredibly easy to deploy applications to an industrial-grade infrastructure. Something that would have itself required a whole separate course to learn how to do, we can now do with just one click; and we can focus instead on building features for our users.

Rails was the first platform that Heroku supported, and Rails developers are still Heroku's primary users. There are many other services, everything from performance monitoring to error reporting to email delivery to A/B testing to analytics, that offer tight integrations with Ruby and/or Rails and make using them hyper-productive for small teams, solo developers, and therefore also for us beginners.

## How are we going to go from zero to Hero(ku)?

To go from _complete beginner_ to _deploying a fully functional application_ is quite a bit of ground to cover. Here's how we're going to do it:

### Editors

As you've already seen, we're going to start by writing Ruby right here in the middle of these readings by using a service called repl.it. This is the lowest-friction way to get started, and will allow us to focus on learning Ruby without having to waste mental energy on things like learning how to navigate the command-line (non-graphical) interface of our computers.

Later, when we're ready to write full-fledged Rails applications, we're going to switch to a different cloud-based programming environment called Cloud9. This will save us the trouble of installing Rails on our own laptops, and we'll get some nice collaboration features from it.

### REPL tests

For now though, I want to talk a little bit more about how the REPLs work. So far, we've been clicking "run ▶" and seeing the return value of the expression of the _last line_ of our programs in the black window (known as the "terminal") at the bottom left; but what if we wanted to see more than just the return value of the last line?

It turns out there's a special method in Ruby called `puts` (short for "put string") that will print out values to the terminal. Try running the following:

<iframe frameborder="0" width="100%" height="600px" src="https://repl.it/student_embed/assignment/3047594/4899c0331ceb9b7b0cd60032e0bb0e0d"></iframe>

I'm also going to start including challenges in the REPLs that you have to pass. Try this one to see how it works:

<iframe frameborder="0" width="100%" height="600px" src="https://repl.it/student_embed/assignment/3047597/c3646e3314f9eaa6aedda51dfdc8d59b"></iframe>

If you got the tests to pass and submitted your solution successfully, you should now see something like this (the below is a screenshot, not a real REPL):

![](/assets/solution-submitted.png)

Whenever you've submitted a solution, you'll be able to click "See Model Solution" to see one that I've written. Remember that there's no single "correct" solution — if yours worked, then it's just as good as mine!

### Exceptional things about puts

The challenge above could be solved with the following:

```ruby
puts("HELLO WORLD!")
```

I wanted to point out a couple of things the code above.

First, you may have noticed that we aren't using the primary syntax of `object.method`; it's just `method` all by itself. Without going into too much detail, the reason for this is known as "syntactic sugar" — Ruby includes some niceties that allow us to type less. In reality, under the hood, it is always `object.method` going on[^self_puts].

[^self_puts]:
    Okay fine, if you _must_ know the fully written out form of it, it's

    ```ruby
    self.send(:puts, "HELLO WORLD!")
    ```

    Happy now? You can try it out in a REPL; and we'll leave at that, for now.

Second, when we discussed [Arguments](https://chapters.firstdraft.com/chapters/754#arguments-are-inputs){:target="_blank"}, we said that they always come within parentheses. That's not quite true. Ruby allows you to, optionally, omit the parentheses; so the following will also work:

```ruby
puts "HELLO WORLD!"
```

And when you are roaming the internet, you will see this style often, especially in conjunction with `puts`. My advice to you is: you can drop the parentheses when you are `puts`ing, but other than that, always include them. They help to keep things clear, and they prevent order-of-operations errors.

But yes, I like to `puts` **a lot** while I am programming. One of my fundamental programming principles is **make the invisible visible** — don't try to _guess_ what's going on, find a way to _see_ what's going on. `puts` is an excellent tool for that, so I `puts` like crazy, almost every line sometimes while I am debugging. It can be tedious to wrap every line in parentheses, and it's convenient instead to just pop a `puts ` at the beginning of a line. So in this one case, I give you permission to omit the parentheses around arguments to a method. Enjoy it.

## Onwards

Okay! With that housekeeping out of the way, let's move on and learn more about the Fundamental Classes of Ruby.

# A few program notes

Before we continue, I want to pause and discuss what, why, and how we're going to be learning.

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

Happily, computers are very fast today[^developer_hours], and they can run Ruby just fine; so now we beginners can have the best of both worlds.

[^developer_hours]: From a business perspective, servers are very cheap while developers are _very_ expensive; so pick the language that makes your developers the most productive.

### Ruby on Rails and other libraries

People often ask "What's the best language for task X?" Depending on the task, sometimes a language is particularly well-suited to it technically; but that is very rare. In general, any programming language can do anything that any other language can (unless maybe there is some kind of proprietary lock-out).

However, there's another very important consideration: which language has the largest **community** of developers doing task X? More community means more shared code (known as "libraries", or in Ruby-land, "gems"), more blog posts, more answers when you Google a question, etc.

For example, Python and Ruby are very similar languages in terms of their technical features and performance profile. However, by some historical accident, Python seems to have gathered more of the scientific/data analysis/machine learning community around it, so more open-source libraries in those areas have been developed and shared in Python than in Ruby; and so now Python is the go-to language for those tasks.

On the other hand, for agilely developing database-backed applications, Ruby has a huge and thriving community. In particular, there is an open-source library for building applications called Ruby on Rails ("Rails", for short) that makes small teams incredibly productive. The existence of Rails alone makes the Ruby language a good choice[^rails_relevant] for both startups and beginners.

The philosophy of Rails is "convention over configuration" — it makes a lot of decisions on your behalf, and if you go with the flow, then things "just work". (If you want to fiddle with settings, then of course you can, to your heart's content; but you don't _have to_ spend hours or days doing so before anything will even show up, like you do in most other frameworks.)

There are a lot of other powerful, easy-to-use Ruby libraries that have philosophies similar to Rails. Ruby seems to have gathered a community of developers around it who are more about creating value for our users, and less about [bike shedding](https://en.wiktionary.org/wiki/bikeshedding){:target="_blank"} over technical details[^choose_boring].

### MINASWAN

The programming world at large can be pretty competitive and harsh, but I've found that the Ruby community is very inclusive and welcoming, which is a relief for beginners. Maybe this can also be traced back to Matz; from [his Wikipedia page](https://en.wikipedia.org/wiki/Yukihiro_Matsumoto){:target="_blank"},

> His demeanor has brought about a motto in the Ruby community: "Matz is nice and so we are nice," commonly abbreviated as MINASWAN.

### Heroku and other SAAS

Another way in which the Ruby community's philosophy seems to have manifested is the amazing amount of tooling that exists around it. For example, [Heroku](https://heroku.com) was the first "platform as a service", a layer on top of Amazon Web Services that makes it incredibly easy to deploy applications to an industrial-grade infrastructure. Something that would have itself required a whole separate course to learn how to do — deployment — we can now do with just one click; and we can focus instead on building features for our users.

Rails was the first platform that Heroku supported, and Rails developers are still Heroku's primary users. There are many other services, everything from performance monitoring to error reporting to email delivery to analytics, that offer tight integrations with Rails and make using it hyper-productive.

[^choose_boring]:
    I, personally, agree with this author who prefers boring technologies over cutting-edge ones:

    [https://mcfunley.com/choose-boring-technology](https://mcfunley.com/choose-boring-technology){:target="_blank"}

[^rails_relevant]:
    Here's a longer blog post on why Ruby on Rails is still a good choice in 2019:

    [https://devbrett.com/2019/03/why-i-believe-rails-is-still-relevant-in-2019.html](https://devbrett.com/2019/03/why-i-believe-rails-is-still-relevant-in-2019.html){:target="_blank"}

## Roadmap

We'll start with repl.it in the browser

Then we'll move on to writing programs using Cloud9

We're going to focus on the database first


Then we'll add the interface

The wiring we build can support any client, but we're going to learn HTML for simplicity

We'll see some tricks for making it work well on phones

## Teaching philosophy

- Questions are where answers fit
- Make learning whole

# Getting Started With Gitpod

One of the most painful parts of learning how to program, in the old days, was simply setting up your computer to be able to write and run code. At a minimum, we needed to install:

 - An application to write your code with. Something like Microsoft Word is not be ideal for writing code, since code needs to be _plain text_ (just a series of characters in a file, nothing else) for the computer to understand it. Word is designed to write _rich text_ (for humans) with fonts, colors, sizes, margins, layouts, etc.
 
    Computers come bundled with some plain text editors (Notepad, TextEdit, etc) but they are very basic. We would instead prefer to use powerful tools specifically designed for writing code with like Microsoft's VSCode or JetBrains' RubyMine.

 - Ruby itself. Writing code is not useful on its own if we don't have something to run it with; just like we need a browser installed to interpret `.html` files we need Excel installed to interpret `.xls` files, and we need Photoshop installed to interpret `.ps` files, we need Ruby installed in order to interpret the `.rb` files that we write.

    Not only that, we need the correct _version_ installed. If your computer happened to come with an older version, upgrading to a newer version could be complicated — especially if some other application you use depends on the older version.

There are so many different combinations of hardware, operating systems, previously installed software, permission levels (for example if you are using a work-owned computer), that just getting these things installed would often stop you before you started writing your first program. We can't allow that!

Instead, we're going to use a write our code using a _cloud_ computer. "Cloud" just means that it's a computer that's sitting in someone's warehouse[^datacenter] somewhere, and we rent it from them. It already has all of the software that we need installed on it, and we access it through our browsers. No muss, no fuss!

[^datacenter]: A warehouse full of computers that people rent and connect to via the internet is called a "data center". Some data centers have their own power plants, and some are even earthquake-proofed.

Gitpod.io is a great new service that provides instantaneous, full-fledged cloud development environments from any codebase that is on GitHub.com — which is great, because we (and 98% of other teams) use GitHub to store all of our projects, homeworks, etc. The text editor they provide is based on Microsoft's VSCode — my editor of choice. It will have the exact right version of Ruby, Rails, and everything else we need. And they have a very generous free tier. Great!

 1. Sign up for a [Gitpod.io](https://www.gitpod.io) account. It will ask you to sign in using your GitHub account.
 1. We will create a **workspace** for each project that we work on. Each workspace is based on a GitHub **repository** (i.e., a folder with some code in it).

    For example, here is a repository:

    [https://github.com/appdev-projects/hello.rb](https://github.com/appdev-projects/hello.rb)

 1. To create a Gitpod workspace based on a repo, in the address bar of your browser enter `https://gitpod.io/#` and then the URL of the repo. For example,

    [https://gitpod.io/#https://github.com/appdev-projects/hello.rb](https://gitpod.io/#https://github.com/appdev-projects/hello.rb)

 1. To make that process easier, [Gitpod has a browser extension](https://www.gitpod.io/docs/20_browser_extension/) that you can install if you want to.
 
 1. Typically, we will assign you a project in Canvas. The assignment will include a button that says "Load assignment in a new window". When you click on that button, it will create a fork (i.e. a copy) of the repository (i.e. the folder of code) on your own GitHub account.

    You will then create a Gitpod workspace based on _your_ fork, so that you can save the work that you do back to your own GitHub account. A button to create your Gitpod workspace will appear within the assignment, so usually all you need to do is click on it after clicking "Load assignment in a new window". And then you can get right to work, with the exact right version of all of the project's dependencies ready to go!

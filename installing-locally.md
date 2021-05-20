# Installing locally

[Gitpod](https://www.gitpod.io/){:target="_blank"} and [Codespaces](https://github.com/features/codespaces){:target="_blank"} are a wonderful solution for getting up and running with a project quickly. You don't have to install anything on your own machine, which means you can work from any device — a Chromebook, a work-issued laptop, even an iPad. You can instantaneously dive into any codebase, for example when code reviewing a colleague's PR, without having to worry about Ruby or gem version conflicts with whatever you're working on ("[dependency hell](https://en.wikipedia.org/wiki/Dependency_hell){:target="_blank"}").

But there are drawbacks to using cloud-based development environments, and at some point you're going to want to install Ruby and Rails on your own machine. Some benefits of local installation:

 - You can run `rails new` yourself to generate a brand new application.
 - Your computer probably has vastly more CPU and memory than Gitpod instances.
 - There's no subscription fees for writing code on your own machine.
 - You can work offline, like when you're on an airplane.
 - You can use other text editors, like [JetBrains' excellent RubyMine](https://www.jetbrains.com/community/education/#students){:target="_blank"}.
 - While [it's possible](https://www.gitpod.io/blog/native-ui-with-vnc/) to develop native desktop/mobile applications in Gitpod, it's a little clunky. It's probably better to develop native apps locally.

There are some hassles involved with local installation, which is why we avoided it initially. But, eventually, the time will come to add this tool to your belt. Here are some suggestions for doing so.

## Windows

Installing Ruby is a little bit more complicated on Windows than on MacOS or Linux. This is due to the fact that most Ruby programs are deployed to servers running Linux, and so that is what Ruby is most optimized for. MacOS shares DNA with Linux (both UNIX-based), and MacOS is probably the most common platform among Ruby developers (it's best to have your development environment be as similar to your deployment target as possible), so installation on Linux and MacOS is more battle-tested than on Windows.

That said, installation on Windows has gotten a _lot_ better over the years. Here is what I recommend:

### Simplest: railsinstaller.org

 1. My recommendation for right now is to use the single-click installer offered by railsinstaller.org. Follow the instructions at [installrails.com](http://installrails.com/){:target="_blank"}. (It appears the railsinstaller.org website is down as of this writing, but you can [download the .exe file from GitHub](https://github.com/railsinstaller/railsinstaller-windows/releases){:target="_blank"}).    
 2. At the step where it asks you to install Sublime Text, I recommend instead using [VSCode](https://code.visualstudio.com/) since it is nearly identical to what we've used in Gitpod. Later, you should experiment with [customizing VSCode](https://betterprogramming.pub/vs-code-extensions-for-ruby-on-rails-developers-917474e03e04){:target="_blank"}. Even later, you should try out [RubyMine](https://www.jetbrains.com/community/education/#students){:target="_blank"} and see which you like best.
 
### Better: Windows Subsystem for Linux

Microsoft has done a 180 and fully embraced Linux; to the extent that you can now install Linux within Windows using the new Windows Subsystem for Linux (WSL) feature in Windows 10. It's still in beta and not totally perfect yet, but it's probably something to look into if you insist on writing Ruby on Windows.

[GoRails has a good guide](https://gorails.com/setup/windows/10) for getting set up with WSL + Rails.

### Best: Docker

In the long-run, I believe the best solution for both Windows and Mac will be to [Dockerize your Rails application](https://pragprog.com/titles/ridocker/docker-for-rails-developers/){:target="_blank"}. 

But investing energy in learning Docker is probably not worth it right now. A couple years from now after you have dozens of Rails apps on your machine, all using different version of Ruby and various gems, and you're feeling then pinch of [dependency hell](https://en.wikipedia.org/wiki/Dependency_hell){:target="_blank"}: that will be the time to explore Docker.

## Mac

 - I prefer [iTerm2](https://iterm2.com/){:target="_blank"} over the built in Terminal.app, but the latter is good too.
 - I use [thoughtbot's laptop script](https://github.com/thoughtbot/laptop){:target="_blank"} to install everything I need in one fell swoop, and recommend that you do so also.
 - I also use [thoughtbot's dotfiles](https://github.com/thoughtbot/dotfiles){:target="_blank"}.
 - I recommend [VSCode](https://code.visualstudio.com/) since it is nearly identical to what we've used in Gitpod. Later, you should experiment with [customizing VSCode](https://betterprogramming.pub/vs-code-extensions-for-ruby-on-rails-developers-917474e03e04){:target="_blank"}. Even later, you should try out [RubyMine](https://www.jetbrains.com/community/education/#students){:target="_blank"} and see which you like best.
 - In the long-run, I believe the best solution for both Windows and Mac will be to [Dockerize your Rails application](https://pragprog.com/titles/ridocker/docker-for-rails-developers/){:target="_blank"}. 

    But investing energy in learning Docker is probably not worth it right now. A couple years from now after you have dozens of Rails apps on your machine, all using different version of Ruby and various gems, and you're feeling then pinch of [dependency hell](https://en.wikipedia.org/wiki/Dependency_hell){:target="_blank"}: that will be the time to explore Docker.

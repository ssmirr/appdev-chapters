# Rails Foundry: Requesting code review

Please use the following procedure to request code review as you build out your project(s) during the Rails Foundry:

## Add instructors as collaborators

After you've created your repository, however you do that (perhaps by [using vanilla-rails as a template](https://github.com/appdev-projects/vanilla-rails/generate){:target="_blank"}), please [add the instructors to it as collaborators](https://docs.github.com/en/github/setting-up-and-managing-your-github-user-account/inviting-collaborators-to-a-personal-repository){:target="_blank"}.

Our GitHub usernames are:

 - `jelaniwoods`
 - `pmckernin`
 - `raghubetina`

## Getting code review

### review-requests channel

We'll use the `#review-requests` channel in our Slack to ask each other for code review. We'll also use the GitHub+Slack app to get automated notifications of when instructors request changes, so that we can all learn from each other's feedback.

### Bot integration

#### #review-requests

[`/join` the #review-requests channel](https://slack.com/help/articles/205239967-Join-a-channel){:target="_blank"} and send the following messages, substituting your own repository name instead of `demostudent14/my-awesome-project`:

 - `/github subscribe demostudent14/my-awesome-project`

 - `/github unsubscribe demostudent14/my-awesome-project issues`
 - `/github unsubscribe demostudent14/my-awesome-project pulls`
 - `/github unsubscribe demostudent14/my-awesome-project commits`
 - `/github unsubscribe demostudent14/my-awesome-project releases`

 - `/github subscribe demostudent14/my-awesome-project reviews`

This will make it so that an automated notification is posted to the channel when instructor reviews are submitted, in case other students want to peruse them.

#### #repo-comments

[`/join` the #repo channel](https://slack.com/help/articles/205239967-Join-a-channel){:target="_blank"} and send the following messages, substituting your own repository name instead of `demostudent14/my-awesome-project`:

 - `/github subscribe demostudent14/my-awesome-project`

 - `/github unsubscribe demostudent14/my-awesome-project issues`
 - `/github unsubscribe demostudent14/my-awesome-project pulls`
 - `/github unsubscribe demostudent14/my-awesome-project commits`
 - `/github unsubscribe demostudent14/my-awesome-project releases`

 - `/github subscribe demostudent14/my-awesome-project comments`

This will make it so that an automated notification is posted to the channel whenever any comment is made on a PR or issue. This will be a place to go to see all the discussion that is happening across all of our projects.

### Asking for review

Whenever you're ready for initial code review, or re-review after you've made changes to address earlier reviews, do two things:

 1. Use GitHub's built-in functionality to [request code review in your PR](https://docs.github.com/en/github/collaborating-with-issues-and-pull-requests/requesting-a-pull-request-review)[:target="_blank"] from all three instructors.
 2. Post a message in the `#review-requests` Slack channel asking for review from your fellow students. Usually we say something like "Could use some review on this please 👀: [https://github.com/demostudent14/my-awesome-project/pull/1](https://github.com/demostudent14/my-awesome-project/pull/1)"

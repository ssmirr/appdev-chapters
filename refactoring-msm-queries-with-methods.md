# Refactoring MSM Queries with Methods

This chapter is the companion to [the refactoring-msm-1](https://github.com/appdev-projects/refactoring-msm-1){:target="_blank"}, which is the sequel to the [the msm-queries project](https://github.com/appdev-projects/msm-queries){:target="_blank"}.

## Objective

Our goal is to keep msm-queries working exactly the same way that it was after we finished building it; we're not going to add a thing. Therefore, we'll use the exact same target as before:

[https://msm-queries.matchthetarget.com/](https://msm-queries.matchthetarget.com/){:target="_blank"}

However, our starting point code for this project, refactoring-msm-1, _is_ the code for the target above. But we're going to make the code much more modular and re-usable, while keeping the functionality exactly the same. How? By **defining methods** to encapsulate our querying logic.

So, this might be a good time for you to review the Chapter on how we define Our Own Classes, and in particular how we [define methods within those classes](https://chapters.firstdraft.com/chapters/769#defining-instance-methods){:target="_blank"}. Go ahead, I'll wait!

---



# Storing credentials securely

Many times we have API credentials or other sensitive information that we don't want to paste directly into our code, because then the information would be exposed on GitHub. Unsavory types like to scrape GitHub for sensitive information like API keys and run up huge bills for compromised users.

Instead, we'll store this information in **environment variables**, which means it lives on the computer somewhere separate from our code, and then our code will read the variables to access it.

In Rails, the way to access environment variables is via the `ENV` hash. The `ENV` hash is available to you everywhere — views, controllers, models, `rails console`, rake tasks, etc. The keys in the hash are the names of any environment variables that exist on the computer you're using, and the values are the contents of the variables.

For example, if there was an environment variable on your computer called `zebra` that had a value of `giraffe`, this is how you would access it from within Rails:

```ruby
ENV.fetch("zebra") # => "giraffe"
```

## Creating environment variables

But how do we create the environment variables and store our secrets in them?

### Gitpod

From your Gitpod dashboard
![](/assets/gitpod-dashboard.png)

Click on your icon in the top right corner and select "Environment Variables"


![](/assets/open-env-gitpod.gif)

Next you can click the "+" button to add a new entry and edit the name and value sections.

The "Organization/Repository" entry is to specify which project(s) on Gitpod will have access to this credential. The "Organization/Repository" of **any** Gitpod workspace can be found on the dashboard. 

![](/assets/gitpod-workspace-organization-repo.jpg)


If you want to allow the use of this credential in all workspaces you can set it to `*/*`. 

Stop and Start your workspace again to pick up the new entry. 

You can test to make sure you've configured everything properly by running `rails console` and fetching the key you added.


```ruby
ENV.fetch("ALGORITHMIA_KEY")
```

and we should see output of

```ruby
=> "replace_me_with_your_key"
```

If you're getting a `"Key not Found"` error, make sure that you specified the correct Organization/Repository. If you need to update that on the Environment Variables page, make sure you Stop and Start your workspace again.
You can use this pattern throughout your Rails app to store and use sensitive info but prevent it from showing on GitHub when you push your code up.

## dotenv

If you're not using Gitpod, there's a slightly better way than storing your secrets in your bash profile; the `dotenv-rails` gem.

 - In your `.gitignore` file, make sure that there's a line somewhere with `/.env*`.
 - Make a git commit for the above change, if necessary.
 - Create a file in the root folder of the app called `.env`. Then add your secrets to it; for example,

    ```
    CLOUDINARY_CLOUD_NAME="paste your cloud name here"
    CLOUDINARY_API_KEY="paste your api key here"
    CLOUDINARY_API_SECRET="paste your api secret here"
    ```

    Since `/.env*` is included in the `.gitignore` file, this file doesn't exist as far as git is concerned; so it's safe to put your secrets in it.
    
 - Restart your `bin/server` and you should now be able to access these values from the `ENV` hash, e.g. `ENV.fetch("CLOUDINARY_CLOUD_NAME")`.
 - [Read more about dotenv here](https://github.com/bkeepers/dotenv).

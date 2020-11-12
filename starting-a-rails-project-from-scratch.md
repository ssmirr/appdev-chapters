# Starting a Rails Project From Scratch

## Create your GitHub repository
1. Visit [https://github.com/appdev-projects/base-rails](https://github.com/appdev-projects/base-rails){:target="_blank"}
2. Click on "Use this template"
  ![](/assets/use-this-template.png)
3. In the "Owner" dropdown, select your GitHub organization and choose a name for your project
  ![](/assets/name-app.png)
4. Make sure "public" is selected
  ![](/assets/select-public.png)
5. Click "Create repository from template" 

## Create a Gitpod workspace off of your GitHub repository
- Copy the URL of your GitHub repositroy
  ![](/assets/copy-repo-url.png)
- In the address bar of your browser type: `gitpod.io/#` + your GitHub repository URL
  ![](/assets/create-new-gitpod-workspace.png)
  - Alternatively, if you have the [Gitpod extension](https://chrome.google.com/webstore/detail/gitpod-dev-environments-i/dodmmooeoklaejobgleioelladacbeki?hl=en){:target="_blank"} installed, you can click on the Gitpod button
  ![](/assets/open-repo-in-gitpod.png)


## Differences between base-rails and a plain new Rails app

We've added our usual goodies to the base-rails repository that aren't included automatically in a new Rails app.

### Gems
- [`amazing_print`](https://github.com/amazing-print/amazing_print){:target="_blank"}
- [`annotate`](https://github.com/ctran/annotate_models){:target="_blank"}
- [`better_errors`](https://github.com/BetterErrors/better_errors){:target="_blank"}
- [`binding_of_caller`](https://github.com/banister/binding_of_caller){:target="_blank"}
- [`draft_generators`](https://github.com/firstdraft/draft_generators/){:target="_blank"}
- [`dotenv-rails`](https://github.com/bkeepers/dotenv){:target="_blank"}
- [`faker`](https://github.com/faker-ruby/faker){:target="_blank"}
- [`http`](https://github.com/httprb/http){:target="_blank"}
- [`pry-rails`](https://github.com/rweng/pry-rails){:target="_blank"}
- [`rails_db`](https://github.com/igorkasyanchuk/rails_db){:target="_blank"}
- [`table_print`](https://github.com/arches/table_print){:target="_blank"}
- [`web_git`](https://github.com/firstdraft/web_git/){:target="_blank"}

### Configuration files

- `.gitpod.yml`
- `.pryrc`
- `config/application.rb`
- `config/initializers/active_record/attribute_methods_patch.rb`
- `config/initializers/active_record/relation.rb`
- `config/initializers/active_record/relation/calculations_patch.rb`
- `config/initializers/active_record/relation/delegation_patch.rb.rb`
- `config/initializers/fetch_store_patch.rb`
- `config/initializers/nicer_errors.rb`
- `config/initializers/open_uri.rb`
- `config/initializers/rails_db.rb`
- `config.ru`
- `lib/tasks/auto_annotate_models.rake`
- `Procfile`

More details to come!

# Starting a Rails Project From Scratch

## Create your GitHub repository
1. Visit https://github.com/appdev-projects/base-rails
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
  - Alternatively, if you have the [Gitpod extension](https://chrome.google.com/webstore/detail/gitpod-dev-environments-i/dodmmooeoklaejobgleioelladacbeki?hl=en) installed, you can click on the Gitpod button
  ![](/assets/open-repo-in-gitpod.png)


## Differences between base-rails and a plain new Rails app

We've added our usual goodies to the base-rails repository that aren't included automatically in a new Rails app.

### Gems
- [`amazing_print`](https://github.com/amazing-print/amazing_print)
- [`annotate`](https://github.com/ctran/annotate_models)
- [`better_errors`](https://github.com/BetterErrors/better_errors)
- [`binding_of_caller`](https://github.com/banister/binding_of_caller)
- [`draft_generators`](https://github.com/firstdraft/draft_generators/)
- [`dotenv-rails`](https://github.com/bkeepers/dotenv)
- [`faker`](https://github.com/faker-ruby/faker)
- [`http`](https://github.com/httprb/http)
- [`pry-rails`](https://github.com/rweng/pry-rails)
- [`rails_db`](https://github.com/igorkasyanchuk/rails_db)
- [`table_print`](https://github.com/arches/table_print)
- [`web_git`](https://github.com/firstdraft/web_git/)

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

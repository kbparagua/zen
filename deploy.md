# Deployment


* For small applications, it is sufficient to restart the server, because only a small amount of time is needed to complete the restart. So users will just experience slow page loads while waiting for the server to be up again.


## Capistrano Utility Gems

### `capistrano-bundler`

  - runs bundle on deploy.
  - require in `Capfile`:`require 'capistrano/bundler'`
  - will run bundle before `deploy:updated`.
  - run manually using `cap production bundler:install`.


### `capistrano-rails`
  
  - runs Rails specific tasks.
  - require in `Capfile`
  
    ```ruby
    require 'capistrano/rails/assets'       # compile assets on deploy
    require 'capistrano/rails/migrations'   # run migrations on deploy
    ```

  - run manually:
  
    ```ruby
    cap deploy:migrate
    cap deploy:compile_assets
    ```

### `capistrano-rvm`
  
  - support for rvm.
  - require in `Capfile`: `require 'capistrano/rvm'`.
  - should work without any advance settings.
  - for advanced rvm setup set additional deploy variables (`:rvm_type`, `:rvm_ruby_version`, `rvm_custom_path`)


## Rolling Deploy/Restart

http://vimeo.com/45927323

1. Available in Phusion Passenger Enterprise.
  - meh.
  - $$$

2. Can be achieved using Phusion Passenger + HAProxy
  - HAProxy is a load balancer.
  - Using load balancer is useless if we only have 1 server (where the application is deployed).


3. Available in Unicorn for free.
  - https://github.com/blog/517-unicorn
  - http://stackoverflow.com/questions/12995296/how-to-do-rolling-restart-with-unicorn


## Unicorn: Rolling Restart

In `deploy.rb`:

```ruby

# This is where the actual deployment with Unicorn happens.
namespace :deploy do
  desc "Start the Unicorn process when it isn't already running."
  task :start do
    run "cd #{current_path} && #{current_path}/bin/unicorn -Dc #{shared_path}/config/unicorn.rb -E #{rails_env}"
  end

  desc "Initiate a rolling restart by telling Unicorn to start the new application code and kill the old process when done."
  task :restart do
    run "kill -USR2 $(cat #{shared_path}/pids/unicorn.pid)"
  end

  desc "Stop the application by killing the Unicorn process"
  task :stop do
    run "kill $(cat #{shared_path}/pids/unicorn.pid)"
  end
end

```

## Migration Issues


```ruby
class RemoveNotesFromUsers < ActiveRecord::Migration
  def self.up
    remove_column :users, :notes
  end
end
```

Error after migration: `PGError: ERROR: column "notes" does not exist`


**ActiveRecord caches table columns and uses this cache to build INSERT statements.**

### Solution:
Any migration being deployed should be **compatible** with the **code that is already running**.


### How?

1. Write a patch to make the code compatible with the migration you need to run.
2. Run the migration.
3. Remove the patch.


### Example

1. Write a patch to make the code compatible with the migration you need to run.
  ```ruby
  class User
    def self.columns
      super.reject { |c| c.name == "notes" }
    end
  end
  ```

2. Run the migration.
  `rake db:migrate`

3. Remove any code written specifically for the migration. 
  ```ruby
  class User
    # def self.columns
    #   super.reject { |c| c.name == "notes" }
    # end
  end
  ```

### Workarounds:

- Adding columns
  - Safe for readonly models
  - Safe when there are no constraints on the column

- Removing columns
  - Safe for readonly models
  - Tell AR to ignore the column first

- Renaming columns
  - Not safe
  - First add a new column, then remove the old one
  - When the column is used on SQL queries you’ll need to split this in three steps

- Creating tables
  - Safe

- Removing tables
  - Safe

- Creating indexes
  - Safe only for readonly models
  - Otherwise make sure you create indexes concurrently

- Removing indexes
  - Safe


SEE: http://pedro.herokuapp.com/past/2011/7/13/rails_migrations_with_no_downtime/

## Source

* http://www.codinginthecrease.com/news_article/show/151984
* http://pedro.herokuapp.com/past/2011/7/13/rails_migrations_with_no_downtime/
* http://blog.intercityup.com/a-perfect-minimal-capistrano-deploy-rb-for-rails-and-unicorn-with-rolling-restarts/

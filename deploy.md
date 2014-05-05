# Deployment


* For small applications, it is sufficient to restart the server, because only a small amount of time is needed to complete the restart. So users will just experience slow page loads while waiting for the server to be up again.


## Rolling Deploy

http://vimeo.com/45927323

- available only for Phusion Passenger Enterprise.


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


## Migration Issues



## Source

* http://www.codinginthecrease.com/news_article/show/151984
* http://pedro.herokuapp.com/past/2011/7/13/rails_migrations_with_no_downtime/

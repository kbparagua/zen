# Deployment


* For small applications, it is sufficient to restart the server, because only a small amount of time is needed to complete the restart. So users will just experience slow page loads while waiting for the server to be up again.

## Capistrano Utility Gems

### `capistrano-bundler`

  - runs bundle on deploy.
  - require in `Capfile`:`require 'capistrano/bundler'`
  - will run bundle before `deploy:updated`.
  - run manually using `cap production bundler:install`.


## Source

* http://www.codinginthecrease.com/news_article/show/151984

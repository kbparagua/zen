# Ubuntu 14.04

1. Install Google Chrome.
  ```
  sudo dpkg -i google-chrome-stable_current_amd64.deb
  ```

1. Install Git and rbenv.
  ```
  sudo apt-get install git-core
  git clone https://github.com/sstephenson/rbenv.git ~/.rbenv
  echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bashrc
  echo 'eval "$(rbenv init -)"' >> ~/.bashrc
  git clone https://github.com/sstephenson/ruby-build.git ~/.rbenv/plugins/ruby-build
  ```

1. Install latest ruby.

  ```
  rbenv install -l
  rbenvn install <latest ruby version>
  ```

1. Install Bundler.

  ```
  git clone -- https://github.com/carsomyr/rbenv-bundler.git ~/.rbenv/plugins/bundler
  gem install bundler
  rbenv bundler on
  ```

1. Install Sublime Text 2.

  ```
  sudo add-apt-repository ppa:webupd8team/sublime-text-2
  sudo apt-get update
  sudo apt-get install sublime-text
  ```
  
1. Install PostgreSQL

  ```
  sudo apt-get update
  sudo apt-get install postgresql postgresql-contrib
  
  sudo -i -u postgres
  createuser --interactive
  createdb <username>
  su <username>
  ```

1. Install MySQL

  ```
  sudo apt-get install mysql-server mysql-client
  ```
  
1. Install `mtpfs` for reading phones

  ```
  sudo apt-get install mtpfs
  ```
  
1. Install VLC

  ```
  sudo apt-get install vlc
  ```

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

1. Install Sublime Text 2.

  ```
  sudo add-apt-repository ppa:webupd8team/sublime-text-2
  sudo apt-get update
  sudo apt-get install sublime-text
  ```
  

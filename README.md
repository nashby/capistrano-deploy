Capistrano deploy recipes [![TravisCI](https://secure.travis-ci.org/lest/capistrano-deploy.png?branch=master)](http://travis-ci.org/lest/capistrano-deploy)
=========================

Inspired by https://github.com/blog/470-deployment-script-spring-cleaning.

Quickstart with Git and Rails
-----------------------------

Minimal Capfile for Rails deploy using Git:

```ruby
require 'capistrano-deploy'
use_recipes :git, :rails

server 'server name or ip address', :web, :app, :db, :primary => true
set :user, 'user for deploy'
set :deploy_to, '/deploy/to/path'
set :repository, 'your git repository'
```

To setup:

    cap deploy:setup

Then when you push some changes to git repository simply run:

    cap deploy

Or if you have migrations:

    cap deploy:migrations

To look through the changes to be deployed:

    cap deploy:pending

Multistage
----------

Basic usage:

```ruby
use_recipe :multistage

set :default_stage, :development

stage :development do
  ...
end

stage :production do
  ...
end
```

You can also pass options that allow setting variables and default stage:

```ruby
stage :development, :branch => :develop, :default => true
stage :production,  :branch => :master
```

When branches are specified for stages and git recipe is used
it will automatically select stage based on current local branch.

Bundle
------

Use recipe:

```ruby
use_recipe :bundle
```

And add callback to run `bundle install` on each deploy:

```ruby
after 'deploy:update', 'bundle:install'
```

Passenger
---------

Use recipe:

```ruby
use_recipe :passenger
```

It will automatically do `touch tmp/restart.txt` on each deploy.

Unicorn
-------

Use recipe:

```ruby
use_recipe :unicorn
```

You can setup callback to reload unicorn after deploy is done:

```ruby
after 'deploy:restart', 'unicorn:reload'
```

Whenever
--------

Use recipe:

```ruby
use_recipe :whenever
```

To automatically update crontab file:

```ruby
after 'deploy:restart', 'whenever:update_crontab'
```

You can also clear crontab file with command:

    cap whenever:clear_crontab

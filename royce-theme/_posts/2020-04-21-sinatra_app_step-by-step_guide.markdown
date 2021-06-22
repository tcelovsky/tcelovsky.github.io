---
layout: post
title: "Sinatra App Step-by-Step Guide"
date: 2020-04-21 21:39:14 +0000
permalink: sinatra_app_step-by-step_guide
tags: [Sinatra]
---

_When building my first Sinatra application from scratch I realized that the biggest issue for me was where to start. I was most struggling with creating the structure of the application without the scaffolding of a lab or a gem. That's why I thought it would be most helpful to write my blog post as a step by step guide for building Sinatra app architecture from scratch. Let's dive in._

Sinatra is a Domain Specific Language implemented in Ruby that's designed for creating web applications with ease. Sinatra is lightweight, flexible and provides us with the bare minimum requirements for building simple web applications. Before we even start with our Sinatra application we should decide on the basic requirements for our app. I decided to build a diary app to help all those who are currently self isolating and/or socially distancing due to COVID-19; benefits of keeping a diary or a journal are well documented in managing stress and improving well-being. I knew that I wanted my app to have a log in/out functionality. In addition, I laid out the following basic requirements:

- User Module:
  - Authentication
    - Must have secure password
  - Sign up/register:
    - Name
    - Email
    - Password
  - Log in:
    - Email
    - Password
  - Has many posts
- Post Module:
  - Belongs to a User
  - Create:
    - Title
    - Content
  - Index page
  - Edit page

After laying out similar requirements for our app, we can start setting up its basic scaffolding:

1.  Open your terminal, make a new directory using the name of your app and cd into that directory.
2.  We'll need a Sinatra gem to build this Sinatra app:
    1. Enter `Bundle init` in your terminal to create a Gemfile and open your text editor.
    2. In the Gemfile in your text editor enter the gems we'll need for this app; this is a good opportunity to utilize one of your previously built apps to copy the gems.
       - gem 'sinatra'
       - gem 'sqlite3'
       - gem 'activerecord', :require => "active_record"
       - gem 'rake'
       - gem 'pry'
       - gem 'sinatra-activerecord'
       - gem 'require_all'
       - gem 'bcrypt', '~> 3.1.7'
       - gem 'shotgun'
    3. Run `Bundle` in your terminal.
3.  Create a config.ru file:
    1. Load an environment: `require_relative './config/environment'`
4.  Make config folder and create environment.rb file there. In the environment.rb file:
    1. `Require ‘bundler’`
    2. `Bundler.require`
5.  This is a good time to connect our database - in the same environment.rb file:

        ActiveRecord::Base.establish_connection(

        :adapter => "sqlite3",

        :database => "db/development.sqlite"

        )

6.  Make Rakefile and enter the following in the Rakefile:

    ```
    require_relative './config/environment'

     require 'sinatra/activerecord/rake'

     task :console do

     Pry.start

     end
    ```

````

At this point we want to make sure that our app is set up in GitHub:

1. In your terminal enter `git init`
2. Go to [github.com](http://github.com/), sign in and create a new directory using the name of your Sinatra app.
3. Back in your terminal enter:
    1. `git add .`
    2. `git commit -m 'Initial commit.'`
    3. `git remote add origin [git@github.com](mailto:git@github.com):yourusername/yourappname.git`
        1. Note that Github will provide the above link once you've created a directory in Github.

Here we start building our Sinatra app, it is good practice to do periodic git commits to ensure our code on GitHub is up to date. We know that we'll be using Model View Controller concept as the base for our app:

1. Create an app folder and inside create the following folders:
    1. Controllers
    2. Models
    3. Views
2.  Add `require_all 'app'` to the environment.rb file since we know that our Sinatra app will be housed in the app folder.
3. Within the Controllers folder create ApplicationController file, this is where our ApplicationController class will live:

        class ApplicationController < Sinatra::Base

        end

4. It is good practice to mount a controller as soon as it is created. In the config.ru file enter `run ApplicationController`
5. Out first controller is mounted, let's make sure everything works so far:
    1. In the application_controller.rb file enter the following code:

            get '/' do

            "Hello World!"

            end

    2. Enter Shotgun in your terminal and navigate to the temporary webpage. If you see "Hello World!" on your screen then you know everything is working as intended and we can continue building our app!
6. Create users_controller.rb and posts_controller.rb files/classes in the Controllers folder. Note that the names of these files/classes would match your intended app design. Are you building a Twitter-like app and will have users and posts? Great! Maybe it's a recipe collection and you'll have contributors and recipes? Then change the file names to match that.
    1. All of the controller classes you'll create after application_controller.rb will inherit from ApplicationController. For example, here is my users_controller.rb class:

            class UsersController < ApplicationController

            end

        2. Remember to mount each new controller by entering `use YourControllerNameController` in the config.ru file.

7. We have our basic controller architecture set up, let's go ahead and create our Models. I built a diary app and needed Users and Posts models. Use the following directions as a guide to create your Model classes.
    1. In the Models folder create a user.rb and post.rb files; keep in mind that the names of the files in the Models directory should be singular (i.e. user, post, tweet, recipe, etc.)
    2. All our Model classes will inherit from `ActiveRecord::Base`
    3. Our User class should have the `has_many :posts` characteristic.
    4. Our Posts class should `belongs_to :user` .
    5. We want our users to be able to sign in using a secure password, add `has_secure_password` to the User class.
    6. We also want to enable sessions, so that users don't have to login every time they navigate to a new page:
        1. In the application_controller.rb file enter:

          ```
  enable :sessions

           set :session_secret, 'secret'
````

        2. You can change `:session_secret` above to whatever you like.

8.  Let's set up our database:

    1.  Create db folder.
    2.  In your terminal enter `rake db:create_migration NAME=create_users` keeping in mind that the `NAME` value here should reflect your Models in plural form.
    3.  Navigate to the newly created file in the db folder and add the parameters for the Users table keeping in mind the attributes they'll need to have, such as name, email and password_digest.

            def change

            	create_table :users do |t|

            		t.string 'name'

            		t.string 'email'

            		t.string 'password_digest'

            	end

            end

    4.  Repeat the above steps to create a posts/tweets/tasks etc. table. Remember that posts belong to users, thus the Posts table will have to have a user_id attribute to successfully link each post to a user.
    5.  In your terminal run `rake db:migrate`

We are now ready to start adding some routes to our controllers and start creating our html views. The below discussion is only a basic overview of this process as the goal of this post was to discuss basic Sinatra app architecture.

1. We want to make sure that our ApplicationController knows where our Views are located, add the following to application_controller.rb: `set :views, 'app/views'`
2. The basic structure of our Views folder will include a home.erb file, layout.erb file, a users folder and a posts folder. This is a good time to create all those.
   1. Layout.erb file is the blueprint for all webpages of your application. If all your pages will have the same header/footer, this is the best place to put those. Add `<%= yield %>` where you would like your blueprint layout to yield to code in another Views file.
   2. A home.erb file is used to welcome the user to the app and ask him/her to register or login.
   3. Users and posts folders will contain views specific to users, such as register.erb and login.erb, and views specific to posts, such as new.erb, edit.erb, show.erb and delete.erb.
3. The reason we have multiple controllers is to logically separate controller functionality:
   1. application_controller.rb file will include routes applicable to the entire app, such as a home route and any helper methods.
   2. users_controller.rb file will include routes that deal specifically with users, such as register, login and logout.
   3. posts_controller.rb file will include routes related to our posts, such as index, show, edit, delete, etc.

As a bonus, add some CSS styling to your app! Sinatra automatically looks for CSS within a folder called public. Create a public folder and add style.css file - this is where your CSS will be housed. Navigate to your layout.erb page within Views and add the following line within the `<head>` attribute:

    <link rel="stylesheet" type="text/css" href="style.css">

I hope this guide has been helpful and I am glad if you learned even just one thing. Happy coding!

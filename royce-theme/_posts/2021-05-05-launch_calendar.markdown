---
layout: post
title: "Launch Calendar"
date: 2021-05-05 19:46
permalink: launch_calendar
# tags: [JavaScript, Tips]
# featured_image_thumbnail:
featured_image: assets/images/posts/launch-calendar/spacex--p-KCm6xB9I-unsplash.jpg
featured: true
hidden: true
---

_This post is an overview of how I built a webpage that lists upcoming rocket launches around the world with details on the date, time, rocket and mission for each._

For my [Flatiron School](https://flatironschool.com/welcome-to-flatiron-school/?utm_source=Google&utm_medium=ppc&utm_campaign=12728169839&utm_content=127574231184&utm_term=flatironschool&uqaid=513747011515&CjwKCAjwhMmEBhBwEiwAXwFoEV6tNm3M-Vh9W3Qee6Y6O1ogIZTjsSYSVzsGo9HKE6b7t7VVYgW12xoCQF0QAvD_BwE&gclid=CjwKCAjwhMmEBhBwEiwAXwFoEV6tNm3M-Vh9W3Qee6Y6O1ogIZTjsSYSVzsGo9HKE6b7t7VVYgW12xoCQF0QAvD_BwE) final project I wanted to build something that I would find interesting and fun to work on. I also knew that I did not want to tackle a super complex project as I wanted to make sure that I worked within certain time constraints. After all, the purpose of this project was to help me graduate and to show off the skills I have acquired during my studies. After some reflection I decided that what I would find most exciting right now was building a webpage that would show a list of upcoming rocket launches around the world.

## MVP

There were many features I wanted my webpage to have, but in order to make sure that I actually finish this project, I decided on the following [Minimum Viable Product ("MVP")](https://www.freecodecamp.org/news/minimum-viable-product-between-an-idea-and-the-product/) goals:

- Home page with a welcome message
- A page with a list of upcoming rocket launches
- For each launch list the following:
  - Date of launch
  - Time of launch
  - Rocket type
  - Mission description
- Routes:
  - Home page
  - Index view with a list of launches
  - Show view for each launch
    - Do not intend to have a separate page for each launch
  - About page

At least initially, I also decided on the following stretch goals:

- Calendar functionality for each launch:
  - Decide how to handle time parcing and conversion
  - Decide what to do about launches where date or time are not yet known
- Information about different rockets:
  - A view page for each rocket type with specs and history
- Twitter bot that will tweet out about upcoming launches
- Link to webpages where launches can be viewed live (depending on availability)

## Tech Stack

I knew that I wanted to build my own Rails API to handle the backend logic. The requirements for the frontend were to use React, Redux, HTML and CSS. Here is what I ended up doing:

- Backend:
  - [Rails API](https://guides.rubyonrails.org/api_app.html)
  - [Whenever gem](https://github.com/javan/whenever) used to schedule a custom Rake task (website scraping)
- Frontend:
  - [React](https://github.com/facebook/create-react-app)
  - [Redux](https://redux.js.org/)
  - HTML
  - [Bootstrap](https://getbootstrap.com/docs/3.4/css/) with some custom CSS
  - [Luxon gem](https://github.com/moment/luxon) used to convert date and time in the appropriate format needed for the Add To Calendar button

I made a decision to use Whenever and Luxon gems while I was already working on my project and incorporated them into the existing code base.

## The Building Phase

I find that actually starting the project is the most difficult part. As I was agonizing over the details before ever writing a single line of code, I decided that writing down a step by step plan may help me get started. Below is the basic plan I wrote to get my project done.

### Create GitHub Repository

- Create a new repository in GitHub with .gitignore, README.md and LICENSE files.
- Clone the newly created repository to your computer using `git clone` command followed by the SSH link to the GitHub repository.

### Add .drawio Diagram

- Create a new .drawio file using `touch .drawio` command.
- In the .drawio file create a diagram representing relationships between the backend models using Entity Relation shapes:
  - Include title of each model.
  - Include characteristings of each model.
  - Include relationships between models.

### Backend Setup

- Create Rails API structure by using `rails new` command followed by name of the Rails API:
  - Add `--api` flag after the name to ensure that Rails only includes the necessary folders and capabilities for the API.
  - Add `--database=postgresql` flag to create the Rails API with Postgres database, instead of the default SQLite3.

_For this project, I entered the following in my terminal: `rails new backend --api --database=postgresql`._

**Note.**
`rails new` command will generate a new Rails repository that will include .git folder. In order to ensure that both the frontend and backend can be stored in the same repository on GitHub (in two separate folders), you'll have to delete this .git file as it will prevent you from pushing your new backend repository to GitHub:

- cd into the new Rails repository just created.
- In your terminal enter `rm -r .git`
- cd back to the top folder of your project
- Ensure that the items listed in the .gitignore file at the root of your project are prefaced with the name of your backend repository. For me this meant adding 'backend' at the front of each item listed in the .gitignore file.

### Backend Setup (Continued)

- cd into the new backend directory just created.
- Navigate to the gemfile and add gem 'nokogiri'. [Nokigiri](https://github.com/sparklemotion/nokogiri) gem will help us with scraping and parsing.
- Uncomment gem 'rack-cors' - his will allow [Cross Origin Resource Sharing (CORS)](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing) in the API. CORS is a security feature that prevents API calls from unknown origins.
- Add gem 'active_model_serializers' to the gemfile. Serialization is the process of converting data into a format that can be transmitted across a computer network and reconstructed later. Backend and frontend of this project will make requests to each other across the interwebs.
- Run bundle install.
- Inside config/initializers/cors.rb file uncomment the following code:

```
Rails.application.config.middleware.insert_before 0, Rack::Cors do
  allow do
    origins '*'

    resource '*',
      headers: :any,
      methods: [:get, :post, :put, :patch, :delete, :options, :head]
  end
end
```

Inside the allow block, origins '\*' means that requests from all origins are allowed. This can be changed to only allow requests from the address of the frontend repo - localhost:3000 for example.

**Note.**
You may wish to create a custom Rake task to expedite the process of dropping, creating, migrating and seeding the database by using a single command. To do so, navigate to the lib directory and create a new file with .rake extension (I named my file dcms.rake). Inside thew newsly created file add the following code:

      ```
      namespace :db do
        task :dcms do
         desc 'Drop, Create, Migrate and Seed the Database'
          Rake::Task["db:drop"].invoke
          Rake::Task["db:create"].invoke
          Rake::Task["db:migrate"].invoke
          Rake::Task["db:seed"].invoke
          puts 'Database dropped, created, migrated and seeded.'
        end
      end
      ```

The above code will invoke each of the Rake tasks in sequence (drop, create, migrate, seed) when running command `rake db:dcms` and will put out "Database dropped, created, migrated and seeded." message when the task has been completed.

### Frontend Setup

- From the main directory of your app, run `npm init react-app` command followed by the desired name for the frontend diretory.

_For this project, I entered the following in my terminal: `npm init react-app frontend`._

- cd into the new frontend directory just created.
- Create src folder, this is where most of the frontend logic will live.
- cd into the src folder and create folders for your components, containers, reducers, actions, styles.

I find that once I have the basic set up of the backend and the front, the coding comes easier. Don't forget to consistently test your code as you go. I would recommend navigating to the backend directory and running `rails s` command in your terminal to start the Rails server. Then I would open up a new terminal window and navigate to the frontend directory, run `npm start` in the terminal to start the server. Having both servers runnig helps me test my code as I go. It's also really exciting to see your project grow and develop during this process!

I hope you find the above overview helpful and feel free to check out my [code](https://github.com/tcelovsky/launch-calendar)!

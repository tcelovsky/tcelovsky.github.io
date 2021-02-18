---
layout: post
title: "Writing Lists"
date: 2021-02-10 14:00
permalink: writing_lists
---

_My Rails API with Javascript Project_

For my Rails API with Javascript project I decided to build a single page web app that allows a user to create multiple lists and add items to those lists. For example, a user may create To Do list, Grocery Shopping list, Travel Destinations list, etc. A user is able to add and delete the items on each list. A user is also able to create and delete the lists.

This app is built using Rails API for the backend and JavaScript, HTML and CSS for the frontend. Rails API was created with Postgres database, instead of the default SQLite3.

My biggest question when building this app was: "Where do I begin?" So with that in mind I decided to write down the steps of building a Rails API with Javascript app. I hope you find this list helpful!

### Before Starting

First things first, figure out what you want to build and what functionality you want your app to have. It helps to write these things down and even draw a diagram or a flow chart. This very basic flow chart can later be added to your project as a Drawio diagram.

### Create GitHub Repository

1. Create a new repository in GitHub with .gitignore, README.md and LICENSE files.
2. Clone the newly created repository to your computer using `git clone` command followed by the SSH link to the GitHub repository.

### Add .drawio Diagram

1. Create a new .drawio file using `touch .drawio` command.
2. In the .drawio file create a diagram representing relationships between the backend models using Entity Relation shapes:
   1. Include title of each model.
   2. Include characteristings of each model.
   3. Include relationships between models.

### Setup the Backend

1. Create Rails API structure by using `rails new` command followed by name of the Rails API:

   1. Add `--api` flag after the name to ensure that Rails only includes the necessary folders and capabilities for the API.
   2. Add `--database=postgresql` flag to create the Rails API with Postgres database, instead of the default SQLite3.

      _For this project, I entered the following in my terminal: `rails new backend-rails-api --api --database=postgresql`._

      **_Note:_**
      `rails new` command will generate a new Rails repository that will include .git folder. In order to ensure that both the frontend and the backend can be stored in the same repository on GitHub (in two separate folders), you'll have to delete this .git file as it will prevent you from pushing your new backend repository to GitHub:

      1. cd into the new Rails repository just created.
      2. In your terminal enter `rm -r .git`
      3. cd back to the top folder of your project
      4. Ensure that the items listes in the .gitignore file at the root of your project are prefaced with the name of your backend repository. For me this meant adding 'backend-rails-api' at the front of each item listed in the .gitignore file.

### Setup the Backend (continued)

2. cd into the new folder just created.
3. Navigate to the gemfile and uncomment gem 'rack-cors'. This will allow [Cross Origin Resource Sharing (CORS)](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing) in the API. CORS is a security feature that prevents API calls from unknown origins.
4. Add gem 'active_model_serializers' to the gemfile. Serialization is the process of converting data into a format that can be transmitted across a computer network and reconstructed later. Backend and frontend of this project will make requests to each other across the interwebs.
5. Run bundle install.
6. Inside config/initializers/cors.rb file uncomment the following code:

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

### Code the Backend

1. Use Rails resource generator to create resources:

```
rails g resource list title
rails g resource list_item content list:references
```

This will create two migrations, two models, and two empty controllers.

2. Add seed data:
   list_a = List.create(title: "To Do")
   list_b = List.create(title: "Grocery Shopping")

   list_item_a = ListItem.create(list: list_a, content: "Pick up dry cleaning")
   list_item_b = ListItem.create(list: list_a, content: "Clean")
   list_item_c = ListItem.create(list: list_a, content: "Finish work project")

   list_item_d = ListItem.create(list: list_b, content: "Milk")
   list_item_e = ListItem.create(list: list_b, content: "Eggs")
   list_item_f = ListItem.create(list: list_b, content: "Beans")

3. Run `rails db:create` to create the database.
4. Run `rails db:migrate` to migrate the database.
5. Run `rails db:seed` to seed the database.

**_Note_**
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

### Code the Backend (continued)

6. Enter `rails c` in the terminal to drop into the Rails console and confirm that the seed data was populated correctly and model relationships are correct.
7. Navigate to app/controllers/lists_controller.rb and add controller actions:

```
    def index
        lists = List.all
        render json: lists, include: [:list_items]
    end

    def show
        list = List.find(params[:id])
        render json: list, include: [:list_items]
    end
```

8. Navigate to app/serializers/list_serializer.rb and add `had_many :list_items` attribute.
9. Start Rails server by entering `rails s` in your terminal and navigate to localhost:3000/lists in your browser. Confirm that JSON is rendered correctly on the page.
10. With the Rails server running, navigate to localhost:3000/lists/1 in your browser. Confirm that JSON is rendered correctly on the page.
11. Navigate to app/controllers/lists_controller.rb and add create, update and destroy controller actions.
12. Navigate to app/controllers/list_items_controller.rb and add controller actions:

```
    def index
        list_items = ListItem.all
        render json: list_items
    end

    def show
        list_item = ListItem.find(params[:id])
        render json: list_item
    end
```

13. With the Rails server running, navigate to localhost:3000/list_items in your browser. Confirm that JSON is rendered correctly on the page.
14. With the Rails server running, navigate to localhost:3000/list_items/1 in your browser. Confirm that JSON is rendered correctly on the page.
15. Navigate to app/controllers/list_items_controller.rb and add create, update and destroy controller actions.

### Setup the Frontend

1. cd to the root folder of your project.
2. Create frontend directory by using `mkdir` command followed by the desired name for the frontend directory.

_For this project, I entered the following in my terminal: `mkdir frontend-js`._

3. cd into the newly created directory.
4. Create HTML file by entering `touch index.html` in the terminal.
5. Create source and styles folders by entering `mkdir src styles` in the terminal.
6. Navigate to index.html and add the basic HTML architecture:

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Rails API with JS Project</title>
    <link rel="stylesheet" href="./styles/styles.css">
</head>
<body>

</body>
</html>
```

7. Update the title line in index.html to the desired title.
8. Open index.html in the browser and confirm the window loads.
9. Create new JavaScript file by entering `touch src/index.js` in the terminal.
10. In the index.html file link the JavaScript file to the HTML page by adding the following before the closing </body> tag:

```
<script type="application/javascript" src="src/index.js"></script>
```

11. Confirm that JavaScrip file was correctly linked to the HTML page by adding `console.log("JS linked to HTML")` to index.js file; then refresh the HTML page in the browser and confirm that the output is displayed in the JavaScript console.

### Code the Frontend

1. Add the following within the `<body>` tag of index.html to create basic setup for the webpage:

```
  <header>
    <h2>My Lists</h2>
  </header>
  <main>
    <div class="container">
        <div class="new-list-container">
          <form id="new-list-form">
            <input type="text" name="list-title" id="new-list-title">
            <input type="submit" value="Add List">
          </form>
        </div>
        <div class="lists-container">

        </div>
    </div>
  </main>
```

3. Test that frontend and backend are linked correctly by adding a simple fetch request to index.js; then refresh HTML page in the browser and confirm that JSON data is rendered in the JavaScript console:

```
const BASE_URL = "http://localhost:3000"
const LISTS_URL = `${BASE_URL}/lists`

fetch(`${LISTS_URL}`)
  .then(response => response.json())
  .then(parsedResponse => console.log(parsedResponse));
```

4. cd into src folder and create adapters and components directories by entering `mkdir adapters components` in the terminal.
5. Add the following to index.js file: `const app = new App()`. The index.js file has only one responsibility - creating the new App object.
6. In the src/adapters folder create listsAdapter.js file. This adapter will be responsible for communicating with the rails API backend.
7. Add the following consts to listsAdapter.js file:

```
const BASE_URL = "http://localhost:3000"
const LISTS_URL = `${BASE_URL}/lists`
const LIST_ITEMS_URL = `${BASE_URL}/list_items`
```

8. Create listsAdapter class and add the following code to listsAdapter.js file:

```
class ListsAdapter {
    constructor() {
        this.listsUrl = LISTS_URL
        this.listItemsUrl = LIST_ITEMS_URL
    }

    getLists() {
        return fetch(this.listsUrl).then(res => res.json())
    }
}
```

9. cd into src/components folder and create the following files: app.js, list.js and lists.js by running `mkdir app.js list.js lists.js` command in the terminal.
10. Create the App class in app.js:

```
class App {
    constructor() {
      this.lists = new Lists()
    }
}
```

The idea is that index.js will get loaded and will call `new App()`, which will run the App constructor function. The App constructor will set a property on the newly created app called lists that points to a new instance of the Lists object.

11. Create new Lists class in lists.js. This Lists Class will communicate with the Lists Adapter and will render the lists on the page.

12. Navigate to list.js and create a new List class. The point of the List class is to house all the HTML and DOM manipulation logic related to lists.

13. Create a new file "listItem.js" in the "components" folder. The point of the ListItems class is to house all the HTML and DOM manipulation logic related to list items.

14. Next, focus on add list and add list item functionality of the app keeping in mind that listsAdapter.js houses all the logic related to communicating with the backend (such as `fetch` requests).

15. Next, focus on delete list and delete list item functionality of the app.

Continue checking the functionanlity of your app as you go.

### Add Styles

1. In the styles folder create file styles.css
2. cd into styles.css
3. Add css styles you desire.

Feel free to check out my [GitHub repository](https://github.com/tcelovsky/rails-api-js-project) for this project, I hope this helps you when you are building your own projects.

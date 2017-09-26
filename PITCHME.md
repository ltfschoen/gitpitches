## Twitter Clone

* [Gitpitch URL](https://gitpitch.com/ltfschoen/gitpitches/twitter_mvc)

### Why use UML diagrams?

1. Visually design classes and relationships
2. Communicate DB structure with team
3. Implementation versions in code. Translate Entities into classes
4. Migrate DB schema into DB tables
5. [Link](creately.com/blog/diagrams/uml-diagram-types-examples/) (UML Diagram different types)

---

### What Entities and Attributes to use?

1. Generate User Stories
  * *As user I can view list of users*
  * *As user I can view list of tweets from a user*
  * *As user I can edit user*
  * *As user I can destroy user*
2. Identify Entities and Attributes - **Users, Tweets**
3. Identify Relationships between Entities
  * *User has many Tweets*, *Tweets belong to User*

---

### How to use Class Diagrams?

1. Sign-up to [Creatly](https://creately.com)
2. Create “Entity Relationship Diagram” schema Template.
3. Read ERD tutorial. [Link](creately.com/blog/diagrams/er-diagrams-tutorial/)

![Press Down Key](assets/down-arrow.png)

+++

4. Determine CRUD Resources
*User has many Tweets. Tweets belong to User.*

Twitter ERD
![Image-Absolute](assets/twitter_erd.png)

---

### How to do "relational" DB Modelling?

1. Create “Database Diagram“ schema Template
2. Read “Database Modelling” tutorial. [Link](creately.com/blog/diagrams/database-modeling-basics/)

Twitter DB Model
![Image-Absolute](assets/twitter_db_model.png)

---

### How to create Static Pages?

1. Create app and static pages. Skip Minitest files.
  ```bash
  rails new twitterclone —database=postgresql --skip-test;
  cd twitterclone;
  rails generate controller Pages home about \
      --stylesheet-engine=scss \
      --javascript-engine=js
  ```
2. Run server
  ```bash
  rails server
  ```
3. Open website
  ```bash
  open -a "Google Chrome" http://localhost:3000/pages/home
  ```

![Press Down Key](assets/down-arrow.png)

+++

4. Review routes
  ```bash
  rails routes
  ```
5. Change routes
  ```ruby
  root to: 'pages#index'
  get '/pages/index', to: 'pages#index'
  ```

---

### How to create Scaffold a Resource?

1. Scaffold generates Model, View, Controller (MVC) with CRUD Actions, Routes, DB Migration, Tests, JavaScript, and CSS template for a RESTful app
  ```bash
  rails g scaffold User username:string email:string password_digest:string \
    --stylesheet-engine=scss \
    --javascript-engine=js
  ```
2. Controller generated with `before_action`, which executes a given method (i.e. `set_event`) before specified Actions (i.e. `index, new, create, show, update, edit, destroy`). [Link](http://guides.rubyonrails.org/action_controller_overview.html)

![Press Down Key](assets/down-arrow.png)

+++

3. Controller generated with Whitelisted (aka [Strong Parameters](http://edgeguides.rubyonrails.org/action_controller_overview.html#strong-parameters)). Security to protect updating sensitive Model attributes
4. Model created
5. Routes `resources :users`

![Press Down Key](assets/down-arrow.png)

+++

6. [ActiveRecord Migration](http://edgeguides.rubyonrails.org/active_record_migrations.html) generates different versions of DB Schema using Ruby DSL instead of raw SQL. ActiveRecord version control history structure of DB stored in `schema.rb`. Default primary key column `id` added implicitly and timestamps for `created_at` and `updated_at` (updated by Active Record).
  * Note: Before Migration no User table exists in DB
  * Note: After Migration reversal with Active Record using `rollback` to remove User table from DB
  * Note: `up` and `down` may be used instead of `change`

---

### How to create Database and use Migrations?

1. Create DB and perform ActiveRecord Migration
  ```bash
  open -a "Postgres"
  rails db:create db:migrate
  ```
2. Run PostgreSQL Shell. Show Help. Show DB list. Switch DB. Show Tables list. Query Table. Quit
  ```bash
  psql
  \h
  \l
  \c twitterclone_development
  \d
  SELECT * FROM users;
  \q
  ```

![Press Down Key](assets/down-arrow.png)

+++

3. Drop DB
  ```bash
  rails db:drop
  ```
4. Recreate DB
  ```bash
  rails db:create db:migrate
  ```

---

### Rails Console and DB Console

* Open Rails Console (use `rails c --sandbox` to [not save changes](http://guides.rubyonrails.org/command_line.html))
  ```bash
  rails console
  ```
* Open DB Console (uses configuration in database.yml)
  ```bash
  rails db
  ```

---

### List View of Users and Create User

1. Go to link below to load /views/users/index.html.erb. Observe Server logs and [Convention over Configuration](http://rubyonrails.org/doctrine/):
  ```
  open -a "Google Chrome" http://localhost:3000/users
  ```
2. Create New User in Browser

---

### Add Migration to Update User Table with Image Attribute

1. Generate Migration to add image attribute/column to user table
  ```bash
  rails generate migration AddImageToUser image:string
  ```
2. Migrate DB Update
  ```bash
  rails db:migrate
  ```

---

### Migrate Seed Data

1. Create a hash in /migrate/seeds.rb
  ```ruby
  User.create(
    username: 'luke',
    email: 'ltfschoen@gmail.com',
    password_digest: 'luke',
    image: 'https://upload.wikimedia.org/wikipedia/en/0/02/Homer_Simpson_2006.png'
  )
  User.create(
    username: 'suess',
    email: 'drsuess@abc.com',
    password_digest: 'suess',
    image: 'https://i.pinimg.com/736x/aa/e5/a7/aae5a7c98714612266776238b05a1582--art-challenge-dr-suess.jpg'
  )
  ```

![Press Down Key](assets/down-arrow.png)

+++

2. Migrate Seed Data
  ```bash
  rails console;
  User.count;
  exit;
  rails db:seed;
  rails console;
  User.count;
  User.all.first;
  ```

---

### Update Front-End User List View using Embedded Ruby

1. Update /views/users/index.html.erb
  ```ruby
  <th>Image</th>
  ...
  <td><p><img class='user-images' src='<%= user.image %>'></p></td>
  ```
2. Update /assets/stylesheets/users.scss
  ```css
  .user-images {
    width: 64px;
    height: 64px;
  }
  ```

![Press Down Key](assets/down-arrow.png)

+++

Twitter Users List Images
![Image-Absolute](assets/twitter_users_list_images.png)

---

### [Customise Generators Scaffolding Workflow](http://guides.rubyonrails.org/generators.html#customizing-your-workflow)

1. Add Configuration Options to /config/application.rb to use:
  ```ruby
  config.generators do |g|
    g.orm             :active_record
    g.template_engine :erb                      # i.e. haml, slim
    g.javascript_engine :js                     # i.e. coffee
    g.stylesheet_engine :sass                   # i.e. css, less
    g.test_framework  :test_unit, fixture: true
  end
  ```
  * Benefits: Flags not necessary (i.e. `--javascript-engine=js`)

---

### Add Tweets Scaffold

1. Update /views/users/index.html.erb
  ```bash
  rails g scaffold Tweet message:string user_id:integer
  rails db:migrate
  ```

2. Change 'message' to `, null: false` (database-level validation)

---

### Add ActiveRecord Model Associations. Update Seeds. Experiment with Associations in Rails Console

1. Add Model Associations between User and Tweets
  ```ruby
  class User < ApplicationRecord
    has_many :tweets
  end

  class Tweet < ApplicationRecord
    belongs_to :user
  end
  ```

![Press Down Key](assets/down-arrow.png)

+++

2. Update Seeds
  ```ruby
  Tweet.create(
    message: 'hallo!',
    user_id: 1
  )
  Tweet.create(
    message: 'hello!',
    user_id: 1
  )
  Tweet.create(
    message: 'bye!',
    user_id: 2
  )
  Tweet.create(
    message: 'cya!',
    user_id: 2
  )
  ```

![Press Down Key](assets/down-arrow.png)

+++

3. Migrate Updated Seeds
  ```bash
  rails db:seed
  ```
4. Create Seed using raw SQL and [Foreign Keys](https://www.postgresql.org/docs/9.5/static/tutorial-fk.html) [datatypes](https://www.postgresql.org/docs/9.5/static/datatype.html)
  ```bash
  psql
  \l
  \c twitterclone_development
  \d
  DROP TABLE users;
  DROP TABLE tweets;
  ```

![Press Down Key](assets/down-arrow.png)

+++

  ```bash
  CREATE TABLE users
  (
   id SERIAL4 PRIMARY KEY,
   username VARCHAR(255),
   password VARCHAR(255),
   email VARCHAR(255),
   image VARCHAR(255)
  );

  CREATE TABLE tweets
  (
   id SERIAL4 PRIMARY KEY,
   message TEXT,
   user_id SERIAL4 references users(id)
  );
  ```

![Press Down Key](assets/down-arrow.png)

+++

  ```bash
  INSERT INTO users (username, password, email, image) VALUES ('kermit', 'kermy', 'kerm@it.com','http://cdn.heftig.co/wp-content/uploads/2015/05/65ae356f64281952741c683297edbcb6.jpg');

  INSERT INTO tweets (message, user_id) VALUES ('nice twitter clone',1);
  ```

![Press Down Key](assets/down-arrow.png)

+++

4. Experiment with Associations in Rails Console. Save updates to DB
  ```ruby
  user1 = User.first
  user1.tweets
  user1.tweets.count
  user1.tweets.create(message: "help")
  tweet1 = Tweet.new(id: 1000, message: "eeep")
  tweet1.user
  tweet1.user = user1
  tweet1.save
  ```

  ![Press Down Key](assets/down-arrow.png)

  +++

5. Experiment with other Queries in Rails Console.
  ```ruby
  users = User.all # User.find_each
  users = User.where(username:"luke").order(created_at: :desc)
  user1 = User.find(1)
  ```

---

### Update User Profile view with their Tweets

1. Display List of User Tweets. Update /views/users/show.html.erb
  ```ruby
  <p>
    <strong>Tweets:</strong>
    <% @user.tweets.each do |tweet| %>
      <ul>
        <li><%= tweet.message %> | <%= tweet.updated_at %></li>
      </ul>
    <% end %>
  </p>
  ```

---

### Setup RSpec

1. Go to [RSpec](https://github.com/rspec/rspec), refers to [RSpec-Rails](https://github.com/rspec/rspec-rails)

2. Add RSpec to Gemfile following the [RSpec-Rails](https://github.com/rspec/rspec-rails) guide.
  ```
  group :development, :test do
    gem 'rspec-rails', '~> 3.6'
  end
  ```

3. Install dependencies
  ```
  bundle install
  ```

4. Generate the RSpec ./spec directory and config files .rspec and
spec/spec_helper.rb
  ```
  rails generate rspec:install
  ```

5. Update Rails Configuration Options to /config/application.rb:
  ```
  config.generators.test_framework :rspec, fixture: true
  ```

  ![Press Down Key](assets/down-arrow.png)

  +++

6. Run only Model specifications
  ```
  bundle exec rspec spec/models
  ```

7. Create Model specification to validate User model
  ```
  require 'rails_helper'

  RSpec.describe User, type: :model do
    it "is valid with an email" do
      u1 = User.new(email: "test@test.com")
      expect(u1).to be_valid
    end
  end
  ```

8. Create Model specification to validate Tweet model
  ```
  require 'rails_helper'

  RSpec.describe Tweet, type: :model do
    it "is valid with an email" do
      user1 = User.create!(email: "test@test.com")
      tweet1 = Tweet.create!(message: "first")

      expect(tweet1).to be_valid
    end
  end
  ```

9. Run tests fails "red"
  ```
  Validation failed: User must exist
  ```

9.5. Note: With Devise, need to provide `password` and `password_confirmation`

10. Fix test "green"
  ```
  tweet1 = Tweet.create!(message: "first", user_id: user1.id)
  ```

  ![Press Down Key](assets/down-arrow.png)

  +++

  11. Add sample data (fixtures)
    * Create a new Model to see how sample fixture uses YAML
      ```
      rails g model TestModel
      rails d model TestModel
      ```

    * Replace User with re-usable user fixture spec/fixtures/users.yml [fixture](http://api.rubyonrails.org/classes/ActiveRecord/FixtureSet.html)
      * spec/fixtures/users.yml
        ```
        luke:
          id: 1
          email: luke@test.com
        ```
      * spec/models/tweet_spec.rb
        ```
        fixtures :users

        it "is valid with an email" do
          #user1 = User.create!(email: "test@test.com")
          tweet1 = Tweet.create!(message: "first", user_id: users(:luke).id)

          ...
        ```
      * Links: https://stackoverflow.com/questions/1976832/rails-fixtures-not-loading-with-rspec

        ![Press Down Key](assets/down-arrow.png)

        +++

    * Repeat by replacing Tweets with fixture
      * spec/fixtures/tweets.yml
        ```
        hello:
          id: 1
          message: $LABEL world!
          user_id: 1
        ```
      * spec/models/tweet_spec.rb
        ```
        fixtures :users, :tweets

        it "is valid when associated with a user" do
          #user1 = User.create!(email: "test@test.com")
          #tweet1 = Tweet.create!(message: "first", user_id: users(:luke).id)

          expect(tweets(:hello)).to be_valid
        ```

    ![Press Down Key](assets/down-arrow.png)

    +++

    * NOTE: Use rspec-expectations instead of the below

    * Add RSpec Collection Matchers (i.e. `have`) https://github.com/rspec/rspec-collection_matchers
      * Add to Gemfile
        ```
        gem 'rspec-collection_matchers'
        ```
      * Install dependencies `bundle install`
      * Add to spec/rails_helper.rb
        ```
        require 'rspec/collection_matchers'
        ```

    ![Press Down Key](assets/down-arrow.png)

    +++

    * Model test for presence of Tweet message. Add new test to confirm that a Tweet is deemed invalid when no 'message' is provided. The test will fail "red" if a missing 'message' does not raise an validation error

    * Add additional fixture
      * spec/fixtures/tweets.yml

        ```
        hello_without_message:
          id: 2
          message: nil
          user_id: 1
        ```

      * Add new test to spec/models/tweets_spec.rb
        ```
        it "is invalid without a message" do
          expect(tweets(:hello_without_message)).to have(1).errors_on(:message)
        end
        ```

      * Add to Tweet Model then run tests so they turn "green"
        ```
        validates :message, presence: true, on: :save
        ```

    ![Press Down Key](assets/down-arrow.png)

    +++

    * Add Model validation to check length of Tweet is 140 characters
      * Link: http://guides.rubyonrails.org/active_record_validations.html
      * Validate Length Link: http://guides.rubyonrails.org/active_record_validations.html#length
      * Generate random string in Ruby https://stackoverflow.com/questions/88311/how-to-generate-a-random-string-in-ruby

      * Add tests to spec/models/tweet_spec.rb

      * TODO - Move to Helper and Add Unit Test of the Random String Generator
        ```
        def generate_string_of_length(len)
          rand(36**len).to_s(36)
        end

        it "is invalid when tweet message is too long" do
          too_long_message = generate_string_of_length(141)
          expect(Tweet.new(message: too_long_message)).to have(1).errors_on(:message)
        end

        it "is invalid when tweet message is too short" do
          too_short_message = generate_string_of_length(9)
          expect(Tweet.new(message: too_short_message)).to have(1).errors_on(:message)
        end
        ```

      ![Press Down Key](assets/down-arrow.png)

      +++

      * Issue that the initial test is not granular enough, since previously passing test is now failing since there are two failed validation error messages instead of one
        ```
        1) Tweet is invalid without a message
           Failure/Error: expect(Tweet.new(message: nil)).to have(1).errors_on(:message)
             expected 1 errors on :message, got 2
        ```

      * Experiment in Rails Console to understand
        ```
        tweet1 = Tweet.new(id: 1, message: nil)
        tweet1.valid?
        tweet1.errors.messages
        tweet1.errors.messages[:message].include? "can't be blank"
        ```

      * Share link http://api.rubyonrails.org/v5.1/classes/ActiveModel/Validations.html

      * Update the newly failing test
        * Links: https://relishapp.com/rspec/rspec-rails/v/2-14/docs/model-specs/errors-on

        ```
        it "is invalid without a message" do
          expect(tweets(:hello_without_message).errors_on(:message)).to include("must have at least 10 characters")
        end
        ```

    ![Press Down Key](assets/down-arrow.png)

    +++

    * Add validation of User email uniqueness
      * Apply implementation syntax of validates uniqueness to models/user.rb http://guides.rubyonrails.org/active_record_validations.html#uniqueness
        ```
        validates :email, uniqueness: true, on: [:create, :update]
        ```
      * Open Rails Console and experiment
        ```
        rails c --sandbox
        u1 = User.new(id:1000, email: "a@b.com")
        u2 = User.new(id:1001, email: "a@b.com")
        u1.save
        u2.save

        => User Exists ... ROLLBACK

        u2.errors.messages

        => {:email=>["has already been taken"], :message=>[]}
        ```

      * Update tests in spec/models/user_spec.rb
        ```
        it "is invalid without unique email address" do
          first_user = User.create(id:1000, email: "a@b.com")
          second_user = User.create(id:1001, email: "a@b.com")
          expect(second_user.errors_on(:email)).to include("has already been taken")
        end
        ```

      ![Press Down Key](assets/down-arrow.png)

      +++

    * Discuss `before` and `after`  
      * Link: https://relishapp.com/rspec/rspec-rails/v/2-11/docs/transactions

    ![Press Down Key](assets/down-arrow.png)

    +++

    * Add an Admin attribute to Devise by following instructions https://github.com/plataformatec/devise/wiki/How-To:-Add-an-Admin-Role
      * Generate migration
        ```
        rails generate migration add_admin_to_users admin:boolean
        ```
      * Modify migration. Add `default: false` to the line that adds the admin column to the table.
      * Run migration
        ```
        rails db:migrate
        ```
      * Update Permitted/strong parameters in Users Controller with `:admin`
        ```
        params.require(:user).permit(:username, :email, :image, :admin, :password_digest)
        ```

      ![Press Down Key](assets/down-arrow.png)

      +++

      * Install FactoryGirl
        * Add to Gemfile
          ```
          group :development, :test do
            gem 'factory_girl_rails'
            gem 'faker'
            gem 'capybara'
            gem 'database_cleaner'
            gem 'launchy'
            gem 'selenium-webdriver'
          end
          ```
        * Install dependencies
          `bundle install`

        * Update config/application.rb
          ```
          g.test_framework  :rspec,
            fixture: true,
            view_specs: false,
            helper_specs: false,
            routing_specs: false,
            controller_specs: true,
            request_specs: false
          g.fixture_replacement :factory_girl, dir: "spec/factories"
          ```
        * Mirror the development database schema onto the test database schema
          ```
          rails db:migrate db:test:clone
          ```

        * Create FactoryGirl Factory for Users
        in spec/factories/users.rb
          ```
          FactoryGirl.define do
            factory :user do
              sequence(:email) { |n| "testemail#{n}@example.com" }
            end
          end
          ```
        * Add test to check FactoryGirl factory generates valid user
          ```
          it "has a valid FactoryGirl factory" do
            u1 = FactoryGirl.build(:user)
            expect(u1).to be_valid
          end
          ```
        * Exercise: Create Tweets Factory and check if valid

        * Simplify so only have to write leaner `build(:user)` (as well as `build`, `create`, `attributes_for` (generates hash of attributes, not an object), and `build_stubbed` methods, by adding the following to specs/rails_helper.rb
          ```
          # Simplify calls to factories
          # i.e. build(:user) instead of FactoryGirl.build(:user)
          config.include FactoryGirl::Syntax::Methods
          ```

        * Replace fixture with FactoryGirl factory for Tweets in specs/factories/tweets.rb
          ```
          FactoryGirl.define do
            factory :tweet do
              association :user
              message { "hello world!" }
            end
          end
          ```

        * Add to Models tweet_spec.rb
          ```
          it "has a valid FactoryGirl factory" do
            create(:tweet, message: "hello world!")
            expect(build(:tweet)).to be_valid
          end
          ```

      * Replace FactoryGirl sequences with Faker to
      generate more realistic fake data in spec/factories/users.rb https://github.com/stympy/faker
        ```
        FactoryGirl.define do
          factory :user do
            email { Faker::Internet.email }
          end
        end
        ```

    * Add Controller tests
      * Add Controller Test for User
        ```
        require 'rails_helper'

        RSpec.describe UsersController, type: :controller do
          describe "POST #create" do
            it "redirects to the list of users view" do
              post :create, params: { user: FactoryGirl.attributes_for(:user) }
              expect(response).to redirect_to users_path
            end
          end
        end
        ```

      * Fix failing test by changing Users Controller's "create" Action to:
        ```
        format.html { redirect_to users_path, notice: 'User #{@user.email} was successfully created.' }
        ```

      * Note: If you get error `undefined local variable or method 'root_path'` then you need to add a root to in routes.rb `root 'users#index'`

    * Add Authentication with Devise https://github.com/plataformatec/devise
      * Add to Gemfile  
        ```
        gem 'devise'
        ```
      * Generate Devise initialisers
        ```
        rails generate devise:install
        ```
      * Configure User with Devise modules
        ```
        rails generate devise User
        ```
      * Add flash messages to views/layouts/application.html.erb
        ```
        <nav>
          <p class="notice"><%= notice %></p>
          <p class="alert"><%= alert %></p>
        </nav>
        ```
      * Migrate Devise into Users
        ```
        rails db:migrate
        ```

      * Add to application.html.erb
        ```
        before_action :authenticate_user!
        ```

      * Add Scoped views to initialisers/devise.rb
        ```
        config.scoped_views = true
        ```
      * Generated Devise views for Users
        ```
        rails g devise:views users
        ```

      * Hide sensitive information by means of redirecting the user to the Users list view only showing the quantity of users if the user is not an admin, otherwise if the user is an admin list all the users and their email addresses

        * Replace in Users Controller index Action:
          * BEFORE
            ```
            def index
              @users = User.all
            end
            ```
          * After
            ```
            def index
              if current_user.admin?
                @users = User.all
              else
                @users_count = User.all.count
              end
            end
            ```

        * Modify views/users/index.html.erb
          ```
          <p id="notice"><%= notice %></p>

          <% if current_user.admin? %>

            <h1>Users</h1>

            <table>
              ...
            </table>

            <br>

            <%= link_to 'New User', new_user_path %>

          <% else %>
            <h3>Currently there are <%= @users_count %> registered users.</h3>
          <% end %>

          ```

        * Show Routes
          ```
          rails routes
          ```
          * Identify that http://localhost:3000/users/sign_up is endpoint where users sign up
          * Go to http://localhost:3000/users/sign_out
          * Change in users_controller.rb adding `current_user ||`
            ```
            def set_user
              @user = current_user || User.find(params[:id])
            ```
          * Update routes.rb so can logout via url
            ```
            devise_scope :user do
              get '/users/sign_out', to: 'devise/sessions#destroy'
            end
            ```
          * Add `admin` checkbox to page http://localhost:3000/users/edit
          so Admin user can make other users an Admin in views/users/form.html.erb
            ```
            <% if current_user.admin? %>
              <div class="field">
                <%= form.label :admin %>
                <%= form.check_box :admin, {}, "true", "false" %>
              </div>
            <% end %>
            ```
          * Change an existing user to be Admin using SQL (alternative to Rails Console) https://www.w3schools.com/sql/sql_update.asp
            ```
            update users set admin = true where id = 3;
            ```

          * Try running tests `bundle exec rspec`
            ```
            Devise::MissingWarden:
               Devise could not find the `Warden::Proxy` instance on your request environment
            ```
          * Setup Controller Tests https://github.com/plataformatec/devise/wiki/How-To:-Test-controllers-with-Rails-3-and-4-(and-RSpec)
            * Add to rails_helper.rb
              ```
              config.include Devise::Test::ControllerHelpers, :type => :controller
              ```

---





### References

* [Gitpitch](https://github.com/gitpitch/gitpitch)
* [Markdown cheatsheet](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet)
* [Atom Markdown Preview](https://github.com/atom/markdown-preview) (CTRL+SHIFT+M)
  ```
  apm install markdown-preview
  ```

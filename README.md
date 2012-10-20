To remember

Fantastic IDE: http://redcareditor.com/
Sublime 2: http://www.sublimetext.com/2

Creating an app:
 rails new first_app

Installing a bundle:
 bundle install

Starting the rails server
 rails server
> viewing site = http://localhost:3000/

Scaffolding the users resource (add, edit, delete)
 rails generate scaffold User name:string email:string

Scaffolding the micropost resource 
 rails generate scaffold Micropost content:string user_id:integer

migrating the database
 bundle exec rake db:migrate

Adding validation (for example on the characters for a micropost)
> In app/models/micropost.rb
 validates :content, :length => { :maximum => 140 }

Linking users to microposts (by ID)
"A user has many microposts, a micropost belongs to a user"
> app/models/user.rb
 add via IDA (has_many :microposts)
> app/models/micropost.rb
 has_many :microposts

Using rails console to view the linking of users to microposts
 rails console
 first_user = User.first
 first_user.microposts
 exit

 Generating a test directory by default: (Test::Unit framework)

rails new Learning2 --skip-test-unit

In order to use generate with RSpec, you need to run the RSpec generator command

rails generate rspec:install

Generating a Controller for StaticPages: (including two actions / methods)

rails generate controller StaticPages home help --no-test-framework

routes for the related actions (eg. home & help) > Found in config/routes.rb

Generating integration tests for static pages

rails generate integration_test static_pages

> found in spec/requests/static_pages_spec.rb

Running the test suite: (from spec/requests/static_pages_spec.rb)

bundle exec rspec spec/requests/static_pages_spec.rb

Example of a test on the home page, for both the h1 and title elements.

require 'spec_helper'

describe "Static pages" do

 describe "Home page" do

   it "should have the h1 'Sample App'" do
     visit '/static_pages/home'
     page.should have_selector('h1', :text => 'Sample App')
   end

   it "should have the title 'Home'" do
     visit '/static_pages/home'
     page.should have_selector('title',
                       :text => "Ruby on Rails Tutorial Sample App | Home")
   end
 end
end

dynamic insertion of title based on page visited > via app/views/layouts/application.html.erb

<title>Ruby on Rails Tutorial Sample App | <%= yield(:title) %></title>

Then you need to provide a title in the page desired. > via app/views/static_pages/home.html.erb

<% provide(:title, 'Home') %>

Defining a custom helper method:

>app/helpers/application_helper.rb

#if no page title is defined, and adds a vertical bar followed by the page title if one is defined.

module ApplicationHelper

 # Returns the full title on a per-page basis.
 def full_title(page_title)
   base_title = "Ruby on Rails Tutorial Sample App"
   if page_title.empty?
     base_title
   else
     "#{base_title} | #{page_title}"
   end
 end
end

Then calling this method inside the title bar >app/views/layouts/application.html.erb

<title><%= full_title(yield(:title)) %></title>

Ruby Console

Defining a simple method:

def string_message(string)
 if string.empty?
  "It's an empty string!"
 else
  "The string is nonempty."
 end
end

Using the method:

puts string_message("foobar")

Using blocks in Ruby:

(1..5).each { |i| puts 2 * i }

Using hashes in Ruby:

params = {}
params[:user] = { name: "Michael Hartl", email: "mhartl@example.com" }
params
params[:user][:email]

Using classes in Ruby:

Word class inherit from String

class Word < String     
 def palindrome?
  self == self.reverse
 end
end

Interacting and calling class methods:

s = Word.new("level")                
s.palindrome?
s.length

Ensuring Ruby (html5) is compatible with IE:

<!--[if lt IE 9]>
<script src="http://html5shim.googlecode.com/svn/trunk/html5.js"></script>
<![endif]-->

How to import twitter bootstrap: > Add to gem file

gem 'rails', '3.2.3'

> Install the bundle

bundle install

> Create a file to contain our css

app/assets/stylesheets/custom.css.scss

Example of importing boostrap (and a small bit of CSS) (app/assets/stylesheets/custom.css.scss)

@import "bootstrap";

/* universal */

html {
 overflow-y: scroll;
}

body {
 padding-top: 60px;
}

section {
 overflow: auto;
}

textarea {
 resize: vertical;
}

.center {
 text-align: center;
}

.center h1 {
 margin-bottom: 10px;
}

Adding partials to (app/views/layouts/application.html.erb):

Example for footer:

<div class="container">
  <%= yield %>
  <%= render 'layouts/footer' %>
</div>

Then create a new file (app/views/layouts/_footer.html.erb:

<footer class="footer">
 <small>
   <a href="http://railstutorial.org/">Rails Tutorial</a>
   by Michael Hartl
 </small>
 <nav>
   <ul>
     <li><%= link_to "About",   '#' %></li>
     <li><%= link_to "Contact", '#' %></li>
     <li><a href="http://news.railstutorial.org/">News</a></li>
   </ul>
 </nav>
</footer>

Adding routs for static pages:

get "users/new"
root to: 'static_pages#home'
match '/help',    to: 'static_pages#help'

Using these routes in hyperlinks:

<%= link_to "About", about_path %>

Adding integration tests for links:

click_link "About"
page.should have_selector 'title', text: full_title('About Us')

Linking to an image (html5)

<%= link_to "Sign up now!", signup_path, class: "btn btn-large btn-primary" %>

Including the full_title test helper method

include ApplicationHelper

Generating a user model with two attributes (name, email):

rails generate model User name:string email:string

Running the migration in (db/migrate/[timestamp]create_users.rb):

bundle exec rake db:migrate

Use SQLLite (sourceforge.net/projects/sqlitebrowser/) to open database (db/development/development.sqlite3)

Viewing the user model (app/models/user.rb):

Adding the annotate gem to Gemfile (under group :development do):

gem 'annotate', '~> 2.4.1.beta'

Then install the bunlde:

bundle install

Then finally creating an annotation:

bundle exec annotate --position before

Playing with Active Directory and our newly created user model

rails console --sandbox
-User.new (creating a new blank user)
-user = User.new(name: "Michael Hartl", email: "mhartl@example.com")
-User.create(name: "A Nother", email: "another@example.org")
-user.email (view user email)

User Validation (for the user model (spec/models/user_spec.rb)

basic validation of a field (examples

describe "when name is not present" do
 before { @user.name = " " }
 it { should_not be_valid }
end

describe "when email is not present" do
 before { @user.email = " " }
 it { should_not be_valid }
end

Validating the presence of the name and email attributes (app/models/user.rb)

validates :name,  presence: true
validates :email, presence: true

This tutorial containts validation for presence, email regex, as well as duplication and same passwords.

Creating a real user via rails console into our users database.

User.create(name: "Michael Hartl", email: "mhartl@example.com",
 password: "foobar", password_confirmation: "foobar")

Adding debug information to the site layout: (app/views/layouts/application.html.erb)

<%= debug(params) if Rails.env.development? %>

Creating params to retrive an id: (app/controllers/users_controller.rb)

def show
   @user = User.find(params[:id])
end

Using FactoryGirl to simulate user model objects:

Add to GemFile:

group :test do
  gem 'factory_girl_rails', '1.4.0'
end

Install the bundle:

bundle install

A test using factory girl to test the user show page: (spec/requests/user_pages_spec.rb)

describe "profile page" do
  let(:user) { FactoryGirl.create(:user) }
  before { visit user_path(user) }
  it { should have_selector('h1',    text: user.name) }
  it { should have_selector('title', text: user.name) }
end

Redirecting the BCrypt cost factor in a test environment: (config/environments/test.rb)

# Speed up tests by lowering BCrypt's cost function.
require 'bcrypt'
silence_warnings do
  BCrypt::Engine::DEFAULT_COST = BCrypt::Engine::MIN_COST
end

Defining a gravatar: (app/helpers/users_helper.rb)

# Returns the Gravatar (http://gravatar.com/) for the given user.
def gravatar_for(user)
  gravatar_id = Digest::MD5::hexdigest(user.email.downcase)
  gravatar_url = "https://secure.gravatar.com/avatar/#{gravatar_id}"
  image_tag(gravatar_url, alt: user.name, class: "gravatar")
end

Adding a gravatar to page: (app/views/users/show.html.erb)

<section>
  <h1>
    <%= gravatar_for @user %>
    <%= @user.name %>
  </h1>
</section>

Using form_for helper method to construct a form using object attribtues (app/views/users/new.html.erb)

<div class="span6 offset3">
   <%= form_for(@user) do |f| %>

    <%= f.label :name %>
    <%= f.text_field :name %>

    <%= f.label :email %>
    <%= f.text_field :email %>

    <%= f.label :password %>
    <%= f.password_field :password %>

    <%= f.label :password_confirmation, "Confirmation" %>
    <%= f.password_field :password_confirmation %>

    <%= f.submit "Create my account", class: "btn btn-large btn-primary" %>
  <% end %>
</div>

An action that can handle a failure: (app/controllers/user_controller.rb)

def create
  @user = User.new(params[:user])
  if @user.save
  flash[:success] = "Welcome to the Sample App!"
    redirect_to @user
  else
    render 'new'
 end
end

Creating a partial for displaying form submission error messages: (app/vies/shared/_error_messages.html.erb)

<% if @user.errors.any? %>
 <div id="error_explanation">
   <div class="alert alert-error">
     The form contains <%= pluralize(@user.errors.count, "error") %>.
   </div>
   <ul>
   <% @user.errors.full_messages.each do |msg| %>
     <li>* <%= msg %></li>
   <% end %>
   </ul>
 </div>
<% end %>

And rendering messages (app/views/users/new.html.erb)

<%= render 'shared/error_messages' %>

Generating a session controller and an integration test:

rails generate controller Sessions --no-test-framework
rails generate integration_test authentication_pages

Adding resources to get the standard RESTful actions for sessions: (config/routes.rb)

resources :sessions, only: [:new, :create, :destroy]
match '/signup',  to: 'users#new'
match '/signin',  to: 'sessions#new'
match '/signout', to: 'sessions#destroy', via: :delete

Including the session helper module into the application controller. (app/controller/application_controller.rb)

include SessionsHelper

Generating a remember token:

rails generate migration add_remember_token_to_users

Adding a migration to add a remember_token (db/migrate)

def change
 add_column :users, :remember_token, :string
 add_index  :users, :remember_token
end

Updating the development and test databasebase:

bundle exec rake db:migrate
bundle exec rake db:test:prepare

Before save callback to create the remember token

before_save :create_remember_token

private

 def create_remember_token
   self.remember_token = SecureRandom.urlsafe_base64
 end

Using if-else branching structure inside embedded ruby:

<% if signed_in? %>
 # Links for signed-in users
<% else %>
 # Links for non-signed-in-users 
<% end %>

Adding boostrap javascript library: (app/assets/javascripts/application.js)

//= require bootstrap

Adding a remember token to the database records

$ rails console
>> User.first.remember_token
=> nil
>> User.all.each { |user| user.save(validate: false) }
>> User.first.remember_token
=> "Im9P0kWtZvD0RdyiK9UHtg"

Session controller methods to create and delete a session for users: (app/controllers/sessions_controller.rb)

class SessionsController < ApplicationController

def new
 end

 def create
   user = User.find_by_email(params[:session][:email])
   if user && user.authenticate(params[:session][:password])
     sign_in user
     redirect_to user
   else
     flash.now[:error] = 'Invalid email/password combination'
     render 'new'
   end
 end

 def destroy
   sign_out
   redirect_to root_path
 end

end

Adding a user edit action: (app/controllers/users_controller.rb)

def edit
   @user = User.find(params[:id])
end

Adding a user update action: (app/controllers/user_controller.rb)

def update
   if @user.update_attributes(params[:user])
     flash[:success] = "Profile updated"
     sign_in @user
     redirect_to @user
   else
     render 'edit'
   end
 end

Adding a signed_in_user before filter: (app/controllers/user_controller.rb) Also adding a correct_user before filter to protect the edit/update pages.

before_filter :signed_in_user, only: [:edit, :update]
before_filter :correct_user,   only: [:edit, :update]

  private

   def signed_in_user
     redirect_to signin_path, notice: "Please sign in." unless signed_in?
   end

   def correct_user
     @user = User.find(params[:id])
     redirect_to(root_path) unless current_user?(@user)
   end

Addin a current_user method: (app/helpers/sessions_helper.rb)

def current_user
  @current_user ||= User.find_by_remember_token(cookies[:remember_token])
end
def current_user?(user)
  user == current_user
end

Code to implement friendly forwarding:

def redirect_back_or(default)
  redirect_to(session[:return_to] || default)
  session.delete(:return_to)
end
def store_location
  session[:return_to] = request.fullpath
end

The sessions create action with friendly forwarding: (app/controllers/sessions_controller.rb)

  def create
  user = User.find_by_email(params[:session][:email])
  if user && user.authenticate(params[:session][:password])
    sign_in user
    redirect_back_or user
  else
    flash.now[:error] = 'Invalid email/password combination'
    render 'new'
  end
end

Method to pull all the users from the database: (app/controllers/user_controller.rb)

before_filter :signed_in_user, only: [:index, :edit, :update]
def index
  @users = User.all
end

Using a faker gem (generate fake DB data)

gem 'faker', '1.0.1'
bundle install

A rake task for populating the database with sample users (lib/tasks/sample_data.rake)

namespace :db do
 desc "Fill database with sample data"
 task populate: :environment do
   User.create!(name: "Example User",
                email: "example@railstutorial.org",
                password: "foobar",
                password_confirmation: "foobar")
   99.times do |n|
     name  = Faker::Name.name
     email = "example-#{n+1}@railstutorial.org"
     password  = "password"
     User.create!(name: name,
                  email: email,
                  password: password,
                  password_confirmation: password)
   end
 end
end

Invoking the rake task:

$ bundle exec rake db:reset
$ bundle exec rake db:populate
$ bundle exec rake db:test:prepare

Adding Pagination:

gem 'will_paginate', '3.0.3'
gem 'bootstrap-will_paginate', '0.0.6'
bundle install

Example of the user index with pagination

<% provide(:title, 'All users') %>
 <h1>All users</h1>

 <%= will_paginate %>

 <ul class="users">
   <%= render @users %>
 </ul>

 <%= will_paginate %>

Paginating the users in the index action: (app/controllers/users_controller.rb)

def index
  @users = User.paginate(page: params[:page])
end

Adding admin to database and site:

rails generate migration add_admin_to_users admin:boolean

Adding a boolean admin attribute to users (db/migrate/ts/_add_admin_to_users.rb)

def change
  add_column :users, :admin, :boolean, default: false
end
bundle exec rake db:migrate
bundle exec rake db:test:prepare

Adding a sample data for admin (just first record) (lib/tasks/sample_data.rake)

desc "Fill database with sample data"
task populate: :environment do
  admin = User.create!(name: "Example User",
                       email: "example@railstutorial.org",
                       password: "foobar",
                       password_confirmation: "foobar")
  admin.toggle!(:admin)

Reset the database and re-populate the sample data:

bundle exec rake db:reset
bundle exec rake db:populate
bundle exec rake db:test:prepare

Creating a destroy method for deleting a user: (app/controllers/user_controller.rb)

before_filter :signed_in_user, only: [:index, :edit, :update, :destroy]
before_filter :correct_user,   only: [:edit, :update]
before_filter :admin_user,     only: :destroy
def destroy
  User.find(params[:id]).destroy
  flash[:success] = "User destroyed."
  redirect_to users_path
end
  private
  def admin_user
    redirect_to(root_path) unless current_user.admin?
  end
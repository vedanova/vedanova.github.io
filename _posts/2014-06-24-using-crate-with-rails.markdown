---
layout: post
title:  "Howto: Using Crate Data with Rails"
date:   2014-06-24 14:35
categories: projects
categories:
- tech
- open source
tags:
- ruby
- rails
- crate
published: true
excerpt_short: Quick intro on how to use Crate Data as database backend for Rails
author: Christoph Klocker
---

If you don't know [Crate](https://crate.io) yet, Crate is a document-oriented cluster data store that is easily scalable and comes
with blob support. You simply start one or more instances and they automatically connect to each other and distribute your 
data to the different nodes. If you worked with replication and sharding before you will love the simplicity of it. Crate is open
source so just try it out.

In this guide I want to quickly walk you through getting Crate working with Rails using the 
[ActiveRecord adapter](http://rubygems.org/gems/activerecord-crate-adapter) I wrote.

# Installing Crate

First [download](https://crate.io/download) Crate or install it through your favorite package manager or you just simply execute the 
bash script in a terminal.

    bash -c "$(curl -L try.crate.io)"
   
After downloading and unpacking it, simply start it

    # ./bin/crate
    
And check if it's running by opening the admin interface at

[http://localhost:4200/admin](http://localhost:4200/admin)

Great, now you have Crate up and running we are ready to use it as backend for Rails.

# Setup Rails

First create your new Rails project

    # rails new crate_rails
    
Now lets modify our Gemfile and exchange the sqlite gem with the activerecord-crate-adapter gem
     
     # Gemfile
     # Use crate as the database for Active Record
     gem 'activerecord-crate-adapter'
     
Don't forget to run bundle
  
     # bundle
    
Next modify your database.yml to connect to Crate. Crate does not support different database schemas so you need to have 
a separate Crate instance running on a different port for running your tests 

    default: &default
      adapter: crate
      host: 127.0.0.1
      port: 4200
    
    development:
      <<: *default
    
    test:
      <<: *default
      port: 4209

# Creating a simple recipe app

For this tutorial we gonna create a simple Recipe app. Each recipe should be assigned to one or more categories
and have an ingredients list.
For the categories and ingredients we are going to use Crate's Array data type and for the properties like preparation time or calories
we store them as an object. The aim of this tutorial is not to create a beautiful app, but to demonstrate using Crate as database
and some of it's key features, the array and object data type. Feel free to fork and play around.

Let's use the scaffold to create our recipe app.

    # rails g scaffold recipe title:string description:string categories:array properties:object ingredients:array 
     
We then need to modify the created migration and specify the object schema and schema type and the array type. So the migration looks
like this

    class CreateRecipes < ActiveRecord::Migration
      def change
        create_table :recipes do |t|
          t.string :title
          t.string :description
          t.array :categories, array_type: :string
          t.object :properties, object_schema_behaviour: :strict,
                   object_schema: {preparation_time: :integer, rest_time: :integer, calories: :integer}
          t.array :ingredients, array_type: :string
          # t.timestamps # Disabled due to a bug in AR adapter that does not set the timestamp automatically
        end
      end
    end
    
Lets create a RecipeProperties class next, that we will serialize into the properties object column. The crate AR adapter comes
with the CrateObject module that we can reuse. It provides us with two methods #dump and #load that does the serialization
for us. All we need to do is to make all instance variable accessible and assign them on initialization. We also add the #to_s
method for pretty printing in our views.
    
    require 'active_record/attribute_methods/crate_object'
    class RecipeProperties
      PROPERTIES = [:preparation_time, :rest_time, :calories]
      attr_accessor *PROPERTIES
    
      include CrateObject
    
      def initialize(opts = {})
        @preparation_time = opts[:preparation_time]
        @rest_time = opts[:rest_time]
        @calories = opts[:calories]
      end
    
      # Pretty print
      def to_s
        str = ""
        PROPERTIES.each do |property|
          str << "#{property.to_s.humanize}: #{send(property)}\n"
        end
        str
      end    
    end
    

Next we will add the serialize functionality on the recipe model. At the same time we also add a before_validation 
to set a unique id, as Crate doesn't support auto incrementation.

    class Recipe < ActiveRecord::Base
      before_validation :set_id
    
      serialize :properties, RecipeProperties
    
      private
      def set_id
        self.id = SecureRandom.uuid
      end
    end


That's pretty much it for what we need to get started. Let's tackle the controller and views

# Creating a new recipe

Let's start with simply allowing to post a new recipe. We need to allow to set all properties attributes. Let's modify
the generated view and change the properties div to this

     # view/recipes/_form.html.erb
    <% props =  f.object.properties %>
    <% RecipeProperties::PROPERTIES.each do |property| %>
      <div class="field">
        <%= label_tag property.to_s.humanize %><br>
        <%= text_field_tag "recipe[properties][#{property}]", props.send(property) %>
      </div>
    <% end %>
         
and the div for the ingredients to this

       <div class="field">
         <%= f.label :ingredients %><br>
         <% f.object.ingredients.try(:each) do |ingredient| %>
             <%= text_field_tag "recipe[ingredients][]", ingredient %><br>
         <% end %>
       </div>
       
For the categories we want to use predefined values that are selectable. Lets add a constant to the recipe model

     CATEGORIES = %w(breakfast dinner lunch)
     
And use it in the view
     
     <div class="field">
       <%= f.label :categories %><br>
       <%= f.select :categories, Recipe::CATEGORIES, {:prompt => "Please select"}, multiple: true, size: 4 %>
     </div>  
     
         
In the controller we need to set a new RecipeProperties object for rendering the form, and some empty values for the 
ingredients field as well. (Adding more ingredients using Javascript is up to you, we limit it to 2 for this tutorial)

      # GET /recipes/new
      def new
        @recipe = Recipe.new(properties: RecipeProperties.new, ingredients: ["", ""])
      end

On the controller side we need to parse the properties into the properties object.  

      def create
        @recipe = Recipe.new(recipe_params)
        @recipe.properties = RecipeProperties.new(params[:recipe][:properties])
        respond_to do |format|
          if @recipe.save
            format.html { redirect_to @recipe, notice: 'Recipe was successfully created.' }
            format.json { render :show, status: :created, location: @recipe }
          else
            format.html { render :new }
            format.json { render json: @recipe.errors, status: :unprocessable_entity }
          end
        end
      end
      
We also modify the strong parameters method recipe_params to allow our parameters
      
      def recipe_params
        params.require(:recipe).permit(:title, :description, properties: [:preparation_time, :rest_time, :calories, :difficulty], ingredients: [], categories: [])
      end
      

Now we should be able to create new recipes. Give it a try.

# Updates

We also need to make a small adjustment on the controller update to allow updates of the properties

      def update
        p = recipe_params
        @recipe.properties = RecipeProperties.new(p.delete(:properties))
        respond_to do |format|
          if @recipe.update(p)
            format.html { redirect_to @recipe, notice: 'Recipe was successfully updated.' }
            format.json { render :show, status: :ok, location: @recipe }
          else
            format.html { render :edit }
            format.json { render json: @recipe.errors, status: :unprocessable_entity }
          end
        end
      end

Now we have updates working as well.
    
# Filtering
    
Now after we have all basic CRUD functionality working, lets create a simple filtering to demonstrate Crate's strength of its
native data types (array, object).

Let's start with filtering by category. First we add a scope on the recipe model and then use it in the controller

    #recipe.rb
    scope :by_category, ->(category) { where("'#{category}' = ANY (categories)")}
    
    # recipes_controller.rb
    def index
      @recipes = Recipe.all
      @recipes = @recipes.by_category(params[:category]) if params[:category]
    end

Now we just need to add some links to the index view

    # recipes/index.html.erb
    
    Filter by:
    <% Recipe::CATEGORIES.each do |category| %>
      <%= link_to category, recipes_path(category: category) %>
    <% end %>
    <br/><br/>
    
People are lazy and only want to spend a certain amount of time for preparing their meals, so lets add a filter by preparation time

    # recipes/index.html.erb
    Filter by max preparation time:
    <% [30, 60, 90, 120].each do |time| %>
      <%= link_to "<#{time}",  recipes_path(max_preparation_time: time) %>
    <% end %>
    <br/>
    
In the recipe model we now query after the properties attribute preparation time. Crate lets us access the object attributes
directly in a query. The select would look like this:

    SELECT recipes.* FROM recipes  WHERE (properties['preparation_time'] < 60)
    
So lets create a scope
    
    #recipe.rb
    scope :max_preparation_time, ->(time) { where("properties['preparation_time'] < #{time}")}
    
and use it in the controller
    
    @recipes = @recipes.max_preparation_time(params[:max_preparation_time]) if params[:max_preparation_time]
    
If you refresh the index page you can filter my preparation time. 
    
    
# Summary

In a quick recap: Instead of creating a separate table for defining categories we store them directly as Array on the recipe table.
Properties are stored as an object what helps us to not have an endless set of columns on the recipe table or using a new property
table that we would then join.
    
   
## Code

The working example can be found on Github: 
[https://github.com/vedanova/crate_rails](https://github.com/vedanova/crate_rails)


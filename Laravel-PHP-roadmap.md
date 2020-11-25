# Laravel Road Map

## Prior Knowledge to be aware of
- Understand the MVC Pattern
- CRUD Conventions

## Laravel Tools that we use a lot
- __php artisan__ (factory for all components, in the console take a look on the comands with `$ php artisan` )

## Basic Concepts
- Routing
  - Passing Request Data to Views
  - Route Wildcards 
  - Routing to Controllers
  - Restful Routing
  - Route model Binding
  - Named Routes
- Databases Handling
  - Configure the Connection (.env)
  - Eloquent Model 
  - Migrations (Database Schema versioning)
- Views
  - Templating Engine (Blade)
  - Layouts (Defining and Extending from)
  - Passing Data into Views
  - Laravel Mix and Webpack
- Sessions
- Validation
- Error Handling
- Logging
  - Log Levels

## Console Tool 
  - Artisan Console 
    - Tinker (REPL) 

## Frontend 
- Blade Templates 
  - Layouts (Defining and Extending from)
  - Displaying Data 
    - Curly braces Sintax (Be a aware of convention to coexist with javascript frameworks `{} and @{}`)
  - Control Structures
    - If Statements
    - Switch Statements
    - Loops
    - The Loop Variable
    - Comments
    - PHP
    - The @once Directive
  - Forms
    - CSRF Field
    - Method Field (`@method`)
    - Validation Errors
  - Components
    - Displaying Components
    - Passing Data To Components
    - Managing Attributes
    - Slots
    - Inline Component Views
    - Anonymous Components
    - Dynamic Components
  - Including Subviews
    - Rendering Views For Collections
  - Stacks
  - Service Injection
  - Extending Blade
    - Custom If Statements
- Compiling Assets (Mix)
  - Basic understanding how npm and nodejs works 
    - npm scripts commands
  - Webpack

## Database
  - Configuration `config/database.php`
  - Artisan Console Handling 

## Eloquent ORM 
  - Relationships
  - Database Factories (Useful for developing and testing)
    - Faker Library
  - Collections
    - Useful methods (Not all, there are dozen)
      - filter
      - map
      - flatMap
      - where
      - merge
      - unique
      - pluck
      - collapse

## Architecture Concepts
  - Service Container
  - Facades
  - Service Provider

## Deeper Concepts
  - Mail
  

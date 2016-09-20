![General Assembly Logo](http://i.imgur.com/ke8USTq.png)

# Rails API Single Resource

## Objectives

By the end of this lesson, students should be able to:

-   ?

## Preparation

1.  [Fork and clone](https://github.com/ga-wdi-boston/meta/wiki/ForkAndClone)
    this repository.

## Prerequisites

-   Ruby basics
-   Ruby objects and classes
-   HTTP
-   [`rails-api`](https://github.com/rails-api/rails-api)
-   [`rails`](https://github.com/rails/rails)
-   [`active_model_serializers`](https://github.com/rails-api/active_model_serializers)
-   [`ruby`](https://www.ruby-lang.org/en/)
-   [`postgres`](http://www.postgresql.org)

This is intended for developers to follow along as I build a library API. You
may follow along as I code but I will not slow down or share this code. Later
in this training we will have a code along.

## Status Check

These will be frequent. As developers we want to be meticulous and make sure
we're getting errors where expected as we build our API.

## Demo: [Library-Demo app](https://github.com/ga-wdi-boston/rails-api-library-demo)

## Code-Along: [Clinic-code-along-app](https://github.com/ga-wdi-boston/rails-api-clinic-code-along)

## Lab: [Cookbook-lab-app](https://github.com/ga-wdi-boston/rails-api-cookbook-lab)

## Add Secrets

-  Follow the instructions in that file to set a **different** key for `development` and `test`.
-  *Do not touch `production`*

## Status Check

-  Navigate to `localhost:3000` in chrome.
  -  Do you get a `welcome aboard` page?
  -  Check your server, are there any error messages?

## Routing

In order for our API to respond to GET requests at the `/books` URL,
we'll need to create a Route that specifies what to do
when that type of request comes in.


```ruby
get '/books', to: 'books#index'
```

This tells Rails,
"When you receive a GET request at the URL path `/books`,
invoke the `index` method specified in the BooksController class."

## Status Check

We changed a small bit of code, let's see if anything has changed.

-  Navigate to `localhost:3000` in chrome.

## Controller

We haven't _defined_ a BooksController class yet,
so if we try to access `localhost:3000/books`, we'll get another error.

The purpose of a controller is to handle requests of some particular type.
In this case, we want to create a new controller called `BooksController`
for responding to requests about a resource called 'Books'.

Rails has a number of generator tools
for creating boilerplate files very quickly.

Not all controllers handle CRUD,
but those that do tend to follow the following convention for their routes
and controller actions:

| Action  | What It Does                             | HTTP Verb | URL           |
|:-------:|:----------------------------------------:|:---------:|:-------------:|
| index   | Return a list of all resource instances. | GET       | `/books`     |
| create  | Create a new instance of a resource.     | POST      | `/books`     |
| show    | Return a single instance of a resource.  | GET       | `/books/:id` |
| update  | Update a single instance of a resource.  | PATCH     | `/books/:id` |
| destroy | Destroy a single instance of a resource. | DELETE    | `/books/:id` |

Let's add an `index` method to `BooksController`.

## Status Check

Let's navigate to `localhost:3000/books` and see if our error has changed.

Let's check out server and see if our error message has changed.

## Model

Let's generate a model.

-  The g stands for generate, Rails does a lot of work for us.

-  Let's look at what was generated.

## Status Check

Let's navigate to `localhost:3000/books` and see if our error has changed.

Let's check out server and see if our error message has changed.

## Migrations

Let's migrate.

Let's navigate to `localhost:3000/books` and see if our error has changed.

We can see what appears to be JSON, but there are no books! That's because we
have not added any.

## Active Record

First let's enter our `rails console` so we can make use of our models. This
lets us interact with Rails from the command line.

### What about Curl and Ajax

If we wanted to use Ajax or curl to `create`, `read`, `update` or `destroy` we
will need repeat the process that we used for index.

*Index is one of the (usually) two controller actions that make up `READ` you*
*will need another controller action if you want top show just one item*

### Routing the Rest of CRUD

Remeber how when we first tried to display our JSON on `localhost:3000` we had
to create a route in `config/routes.rb`?

-  This is what in turn triggered the proper controller and controller action.
-  Which in turn interacted with the model
-  Which in turn interacted with the database
-  This cycle then sends information back the way which it came (in this case).

We already did the inital hard work of creating our Books Controller and our
Book Model. We tok care of the database too, so all we have left to do is:

1.   Add proper routes which trigger the correct controller/controller action
1.   Add the corresponding controller action

`:id` is a dynamic segment, it tells rails to expect a piece of information
which goes inside of a *params hash* which will be accessible in the controller.

*Remember how I said Rails does a lot for us*

It's crucial that we use and `:id` dynamic segment otherwise when we make our
show, patch, and delete requests Rails won't know which item in the data store
you're referring to.

Developers often find shortcuts for things they have to do repeatedly.

There is a shorthand way of writing all the routes listed above.

We have our routes, now it's time to create the controlled actions that
correspond to those routes.

### Controller CRUD

We can already get a list of all of our books, but lets write the controller
action that returns a single book.

Let's test it out `localhost:3000/books/1`.

Before we go further we should refactor our controller a bit to make it more
secure and DRY.

1. First We're going to set a `before_action` right below where we open up the
`BooksController`.

Then we have to create the `set_book` method which will just define the instance
variable `@book` as the book that corresponds to the dynamic id passed in,
in the route.

2. Next we want to create a method that will only allow / permit certain keys
(from the key/value pairs being passed in from `create` and `update` requests).

This requires a root key of `book` and will permit the client to send a title
and author as well. *We could also require title/author if desired*

Now that we are secure and we can set a single book, we need to finish our CRUD
actions with `create`, `update` and `delete`

One last thing, our helper methods, like `set_book` and security methods like
`book_params` shouldn't be public or able to be accessed in any way, so we're
going to make them private by adding a line right before the final `end` that
closes our `books_controller`.

### Status Check

Let's test the API by writing the curl request to `create`, `update` and
`destroy` a book.

### A Note on Best Practices

While it is important that you understand how to write a controller and what
each part does as mentioned earlier Rails does a fair amount of work for us.

There is a command that will *generate the model, controller and routes for us.*
It will also create all of the standard crude actions which we wrote by hand.
This command is `rails g scaffold` to create exactly what we wrote you would
type `rails g scaffold book title:string author:string`

Different developers have different opinions on using scaffolding, some think
it's lazy and would rather be sure about everything they are putting into their
code.  Others think it's the ultimate tool for productivity and prevent bugs,
errors and typos.

*I would recommend using scaffolding.*

### Migrations

What if we've gotten this far and realized that not only do books have an
`author` and `title`, but we also want to store some `secret_info`? We don't
have to start from scratch, we just have to change out model and tell it to
update the date store accordingly.  We do this with something called a `migration`.

Let's check `localhost:3000/books`, did anything change?

### Serializers

Let's say that secret info was something that was useful for us, like a password
or SSN or something valuable. We don't want to let anyone who makes a `GET`
request to see that valuable info. That's what serializers are for.

It's easy to think of them as JSON formatters. They control what information can
is sent to the client.

Generate a serializer.

Check `localhost:3000/books`, did anything change?

Let's add some attributes that we want to allow the client to see in
`serializers/book_serializer.rb`

### Done

Good job!

## Additional Resource

-   **[RailsGuides](http://guides.rubyonrails.org/getting_started.html)**
-   **[Official Rails Documentation](http://rubyonrails.org/documentation/)**
-   **[MVC](https://en.wikipedia.org/wiki/Model%E2%80%93view%E2%80%93controller)**
-   **[JSON API](https://thesocietea.org/2015/02/building-a-json-api-with-rails-part-1-getting-started/)**

## [License](LICENSE)

1.  All content is licensed under a CC­BY­NC­SA 4.0 license.
1.  All software code is licensed under GNU GPLv3. For commercial use or alternative licensing, please contact legal@ga.co.

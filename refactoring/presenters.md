---
layout: default
title: Advanced Ruby on Rails
---

# Writing maintainable code

## Presenters 

A _Presenter_ acts between the Model and the View.

## Scenarios

There are a few different scenarios for a Presenter in Rails. Usually, it's where you need interact with your data in a way that ActiveRecord doesn't easily support, but you don't want to clutter up your views with complex Ruby code. 

One example may be grouping blog posts by year and month. Another might be "Object-oriented" helper methods, so instead of `car_price_range(car)`, you might have:

<pre>
class CarPresenter &lgt; Struct.new(:car)
  # If we don't define a method, just return the default
  def method_missing(method, *args)
    return car.send(method, *args) if car.respond_to?(method)
    super
  end

  def price_range
    if price > 500
      "expensive"
    else
      "average"
    end
  end
end

car = CarPresenter.new(car)
puts car.name
puts car.price_range
</pre>

## Postgres Views over Presenters

Another scenario is dealing with complex join views; say you want various aggregate data for sales over the past 3 months, and to display this in a table view.

In such cases, I recommend pushing the work into your database layer. By that I mean "create" a **database view*** .

> In database theory, a view is the result set of a stored query — or map-and-reduce functions — on the data, which the database users can query just as they would in a persistent database collection object. **Wikipedia**

There are several reasons views are preferred, if your database engine supports them:

1. Performance: No data traveling across the wire, except for the final numbers. IO when talking to the database is a bottleneck; moreso the farther apart your database & app servers are geographically.
2. When you push the data back into a database table, you get all of your favorite ActiveRecord features thrown in for free. No need to create a Presenter: just set up your `class MyAggregateView < ActiveRecord::Base; end` and you're done.

### More Information

* [Database views, Wikipedia](https://en.wikipedia.org/wiki/Database_view)
* [Postgres Views Documentation](http://www.postgresql.org/docs/8.3/static/tutorial-views.html)
* [Exploring the Presenter Pattern, Robin Roestenburg](http://www.tamingthemindmonkey.com/2012/04/10/exploring-the-presenter-pattern)
* [Presenter Pattern, Jay Fields' Thoughts](http://blog.jayfields.com/2007/03/rails-presenter-pattern.html) *Note this uses older Rails 2 examples*
* [Model-view-presenter, Wikipedia](https://en.wikipedia.org/wiki/Model%E2%80%93view%E2%80%93presenter)
* [Decorators vs. Presenters, StackOverflow](http://stackoverflow.com/questions/7860301/rails-patterns-decorator-vs-presenter)

**[Next: Command Pattern](commands.html)**

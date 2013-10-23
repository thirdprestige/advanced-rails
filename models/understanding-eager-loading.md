---
layout: default
title: Advanced Ruby on Rails
---

# Working with eager loading

In many cases, ActiveRecord makes improving query performance remarkably easy. Here, we'll learn how.

## The N+1 Problem

<pre>
clients = Client.limit(10)
 
clients.each do |client|
  # Each time we run this line, 
  # we query the database for "SELECT * FROM ADDRESSES WHERE client_id = $"
  #
  # So for 10 clients, we have 11 queries (N + 1 where N = 10)
  puts client.address.postcode
end
</pre>

Of course, 11 queries may not be that bad, depending on the app, how many records you have total, your database indexes, and other problems. If you show 50 records on a page, though, and you have 400,000 records total, you can be in trouble.

## Other Examples

<pre>
@cart = current_user.cart
 
&lt;%= @cart.items.each do |item| %&lt;
  # This is actually 2N+1, believe it or not...
  # we have to look up the product on the item,
  # and then the image on the product
  #
  # Not so great
  &lt;%= image_tag item.product.image.url %&gt;
&lt;% end %&gt;
</pre>


## Solving the N+1 problem

This is generally really easy in Rails.

<pre>
# 2 queries happen here:
# clients    = "SELECT * FROM clients LIMIT 10"
# addresses  = "SELECT * FROM addresses WHERE client_id IN (5, 912, 327, ...)" 
clients = Client.limit(10).includes(:address)
 
# Now we've run our 2 queries (instead of 10), 
# and this is much faster...
clients.each do |client|
  puts client.address.postcode
end
</pre>

## Nested Includes

What about our shopping cart example?  We grabbed the item -> product -> image. Can we grab all the images in one query?

Sure.

<pre>
@cart = current_user.cart
 
&lt;%= @cart.items.includes(:product => :image).each do |item| %&gt;
  # One query for the items,
  # One query for the item products, 
  # One query for the item products images
  # Not so bad, not so bad at all
  &lt;= image_tag item.product.image.url %&gt;
&lt;% end %&gt;
</pre>

## N+1 in Mongoid: The Identity Map Pattern

So that's great for ActiveRecord.  What about Mongoid? 

In `mongoid.yml`: `identity_map_enabled: true`

And then it's basically like ActiveRecord:

<pre>
Band.includes(:albums).each do |band|
  puts band.albums.first.name # Does not hit the database again.
end
</pre>


## More Resources

* [Bullet RailsCast, Bullet identifies N+1 queries for you](http://railscasts.com/episodes/372-bullet?view=asciicast)
* [Eager Loading Associations](http://guides.rubyonrails.org/active_record_querying.html#eager-loading-associations)
* [Identity Map for Mongoid](http://mongoid.org/en/mongoid/docs/identity_map.html)
* [Querying in Mongoid](http://mongoid.org/en/mongoid/docs/querying.html#queries)

## Next

[Scoping](/models/scoping.html)

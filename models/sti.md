---
layout: default
title: Advanced Ruby on Rails
---

# Single-Table-Inheritance

What do we do if we have classes with _identical data_ but _differing behavior_? 

For example, take a Company.  Our business might support Companies, Firms, Studios, and Sole Proprietorships: the same data for type of Company (name, street address, years in business, etc).

But the behavior is different: for a Firm, we might provide NET 30, for a Sole Proprietorship, we charge upfront but also offer a 10% discount to compensate.

We need different classes for these types of data, but ideally we'd just store the data in the same database table.  **Enter Single-Table-Inheritance**

## Setup

Couldn't be easier. 

<pre>
rails generate migration AddTypeToCompanies type:string
rake db:migrate
</pre>

## `type` ?

`type` is a magical column in Rails that stores the class name of the record.

We can change the column ActiveRecord uses by specifying the `inheritance_column`:

<pre>
class Company &lt; ActiveRecord::Base
  self.inheritance_column = 'class_type'
end
</pre>

Convention over configuration would dictate we just use the `type` column generally though.

## Creating the sub-classes

<pre>
class Firm &lt; Company

end

class Studio &lt; Company

end
</pre>

Done and done.

## Pulling from the database

<pre>
Company.first.class # =&gt; Firm
</pre>

## Subclass Storage
  end

I sometimes namespace my child models, like this:

<pre>
class Company &lt; ActiveRecord::Base
  class Firm &lt; Company

  end

  class Studio &lt; Company

end

Company.first.class # =&gt; Company::Firm
</pre>

## More Resources

* [ActiveRecord Single Table Inheritance](http://api.rubyonrails.org/classes/ActiveRecord/Base.html#label-Single+table+inheritance)

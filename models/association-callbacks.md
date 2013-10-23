---
layout: default
title: Advanced Ruby on Rails
---

# Association Callbacks

Callbacks run on `has_many` associations.

Available callbacks:

* before_add
* after_add
* before_remove
* after_remove

<pre>
class Customer &lt; ActiveRecord::Base
  has_many :orders, before_add: :check_credit_limit
 
  def check_credit_limit(order)
    ...
  end
end
</pre>

Stacking callbacks:

<pre>
class Customer &lt; ActiveRecord::Base
  has_many :orders,
    before_add: [:check_credit_limit, :calculate_shipping_charges]
 
  def check_credit_limit(order)
    ...
  end
 
  def calculate_shipping_charges(order)
    ...
  end
end
</pre>

Preventing adding or removing:

<pre>
class Customer &lt; ActiveRecord::Base
  has_many :orders, before_add: :check_credit_limit
 
  def check_credit_limit(order)
    # Does not add order to the lists
    raise CreditCheckServerDownException
  end
end

</pre>


## More Resources

* [Association Callbacks on RailsGuides](http://guides.rubyonrails.org/association_basics.html#association-callbacks)

## Next

[Adding association extensions](/models/association-extensions.html)

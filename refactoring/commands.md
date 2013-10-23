---
layout: default
title: Advanced Ruby on Rails
---

# Command Pattern

> The command pattern is a design pattern used to encapsulate all of the information needed to execute a method or process at a point in time. In a web application, commands are typically used to delay execution of a method from the request cycle to a background processor. **Keith Gaddis, Imperator gem author**

## Example

The **CreditCardsController**:

<pre>
class CreditCardsController &lt; ApplicationController
...
  def create
    @command = CreateCreditCard.new(current_user.cart, request.query_string)

    if @command.execute
      redirect_to credit_cards_url, notice: 'Credit card added'
    else
      redirect_to new_credit_card_url, alert: 'Unable to create card'
    end
  end
...
end
</pre>

<pre>
class CreateCreditCard &lt; Struct.new(:cart, :gateway_confirmation)
  def result_credit_card
    result.credit_card
  end

  def result
    @result ||= Braintree::TransparentRedirect.confirm(query)
  end

  def execute
    return false unless result.success?

    @card = cart.create_credit_card do |card|
      card.card_type       = result_credit_card.card_type
      card.expiration_date = result_credit_card.expiration_date
      card.last_4          = result_credit_card.last_4
      card.token           = result_credit_card.token
      card.user_id         = cart.user_id
    end

    cart.save
  end
end
</pre>


## Real-life Scenario

[Huckberry.com](https://secure.huckberry.com/) is an eCommerce shop that uses [Braintree](https://www.braintreepayments.com/) for payment processing.  The actual application is a custom Ruby on Rails system.

Huckberry uses Braintree's Transparent Redirect system, which means static web forms POST directly to Braintree (who handles PCI compliance), and Braintree then forwards the customer back to Huckberry's systems, along with a token that we can use to confirm whether the request was successful.

This is a bit of a pain to work into ActiveRecord in a clean way—you can't just run `CreditCard.create(params[:credit_card])`, there are a few in-between steps.

And you have to reflect all of this on the UI layer, as well.

## Imperator gem

Keith Gaddis at TicketBud has developed a gem that helps bring ActiveModel features into the Command pattern, so you can still run validations, callbacks, and the like within your commands.

A lot of times Commands are just straight classes, though.

## Advantages

* Testability.
* Mockability
* Queueability: super easy to push Commands into background jobs when they become a bottleneck

## Disadvantages

The major disadvantages are verbosity and a balooning object inventory. You have to explicitly pass in the values you need to work with, so there's more maintenance overhead.  

Or, you can just delegate all of your calls directly to the object you're working with:

<pre>
class CreateOrderTweet &lt; Struct.new(:order, :twitter_token)
  def execute
    Tweet.new(twitter_token).create("I just ordered #{items.pluck(:name).join(',')} for $#{total}!")
  end

  # "items" and "total" now forward on to the "order"
  def method_missing(name, *args*, &amp;block)
    order.send(name, *args, &amp;block)
  end
end
</pre>

Generally, if I'm working with a 3rd-party service (Twitter, Braintree), I place the logic in a Command, otherwise, I use a Concern.

**[Next: Concerns](concerns.html)**

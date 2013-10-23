---
layout: default
title: Advanced Ruby on Rails
---

# Mocks &amp; Custom Matchers

Here are some more advanced features of RSpec.

## Mocks

Mocks are all about setting expectations for a responsibility and ensuring those expecations are met.  They are completely separate from the database layer, so they're blazing fast.

Generally I've seen mocks used, or "fixtures" (such as factory_girl or Machinist), but not both.

### Examples

<pre>
book = double("book")
allow(book).to receive(:title) { "The RSpec Book" }
allow(book).to receive(:title).and_return("The RSpec Book")
</pre>

**Test Spies**

<pre>
invitation = double('invitation', :accept => true)

user.accept_invitation(invitation)

expect(invitation).to have_received(:accept)

# You can also use other common message expectations. For example:
expect(invitation).to have_received(:accept).with(mailer)
expect(invitation).to have_received(:accept).twice
expect(invitation).to_not have_received(:accept).with(mailer)
</pre>

**[Next: Writing Cucumber Tests](tests.html)**



---
layout: default
title: Advanced Ruby on Rails
---

# Writing Cucumber Tests

## Capybara

Capybara is a driver for basically mocking a web browser in Ruby.

## Installation

Add to your Gemfile: 

<pre>
group :test do
  gem 'capybara'
  gem 'capybara-webkit'
  gem 'cucumber-rails', require: false
  gem 'database-cleaner'
end
</pre>

## Issues with OS X

In OS X, installation sometimes fails without the `qt` library.  Homebrew helps us out:

`brew install qt`

More information in [this GitHub issue](https://github.com/thoughtbot/capybara-webkit/issues/521). 

Wrap it all up with `bundle install`, then `rails generate cucumber:install`.

## Examples
 
Example `features/sign_up.feature`:

<pre>
Feature: Sign Up
  In order to attend Retreat
  As a prospective trainee
  I want to give Retreat my information

  Scenario: Sign up for the first time
    Given I am on the sign up page
    And I have entered "bob@example.com" into "Email"
    And I have entered "********" into "Password"
    And I have entered "********" into "Confirm Password"
    When I press "Continue to Registration"

    Then I should see "Welcome to Retreat. Please continue your registration below"
    And my "Email" should be "bob@example.com"
    And I should have no emails

    When I enter "Bob" into "First name"
    And I enter "Smith" into "Last name"
    And I enter "555-5555" into "Home phone"
    And I enter "555-5555" into "Mobile phone"
    And I press "Save and Continue"
    Then my "State" should be "TX"

    When I enter "3001 RR 620 South" into "Street address"
    And I enter "328" into "Apartment"
    And I enter "Austin" into "City"
    And I enter "78723" into "Zipcode"
    And I press "Save and Continue"
    Then my "Birth date" should be "1980-06-01"

    When I choose "male"
    And I select "Highschool/GED" for my "Education"
    And I select "Hispanic/Latin American" for my "Ethnicity"
    And I press "Save and Continue"

    Then I enter "Jess White" into "Emergency contact"
    And I enter "555-5555" into "Emergency phone"
    And I press "Save and Continue"

    Then I select "Married" for my "Relationship status"
    And I enter "Jane Smith" into "Name"

    Then I enter "Tom" into "Child Name"
    And I enter "12" into "Child Age"
    And I press "Save and Continue"

    Then I enter "John Doe" into "Sponsor Name"
    And I enter "john@example.com" into "Sponsor Email"
    And I enter "Barista" into "Occupation"
    And I enter "StarBucks" into "Employer"
    And I enter "Christian" into "Spirituality"
    And I press "Save and Continue"

    Then I enter "awesome" into "Overall health"
    And I enter "unable to scale walls like Spider-man" into "Health limitations"
    And I press "Save and Continue"

    Then I should be on my dashboard
    And I should see "Thank you for completing your Retreat profile"
    And "bob@example.com" should receive an email with subject "Welcome to Retreat, Bob Smith"

    When I open the email with subject "Welcome to Retreat, Bob Smith"
    Then I should see "555-5555" in the email body
    And I should see "3001 RR 620 South" in the email body
    And I should see "328" in the email body
    And I should see "Austin" in the email body
    And I should see "TX" in the email body
    And I should see "78723" in the email body
    And I should see "Highschool/GED" in the email body
    And I should see "Hispanic/Latin American" in the email body
    And I should see "Jess White" in the email body
    And I should see "Married" in the email body
    And I should see "John Doe" in the email body
    And I should see "john@example.com" in the email body
    And I should see "Tom" in the email body
    And I should see "Barista" in the email body
    And I should see "StarBucks" in the email body
    And I should see "Christian" in the email body
    And I should see "awesome" in the email body
    And I should see "Spider-man" in the email body

    When I click the first link in the email
    Then I should be on my dashboard
    And I should see "Your account was successfully confirmed. You are now signed in."
    Then I sign out

    When I wait a few weeks
    And I sign in
    Then I should not see "You have to confirm your account"
</pre>

Example `features/step_definitions/sign_up_steps.rb`

<pre>
Given(/^I am on the sign up page$/) do
  page.visit new_user_registration_path
end

Then(/^I should be on my dashboard$/) do
  current_path.should == dashboard_root_path
end

Then(/^my account should be confirmed$/) do
  current_user.confirmed_at.should_not be_nil
end

When(/^I wait a few weeks$/) do
  Timecop.travel 3.weeks.from_now
end

Then(/^I sign out$/) do
  page.click_link('Sign out')
end

When(/^I (sign|am signed) in$/) do |*args|
  User.last || User.make!

  page.visit new_user_session_path
  page.fill_in 'Email', with: User.last.email
  page.fill_in 'Password', with: 'test1234'
  page.click_button 'Sign in'
end
</pre>


## Handling AJAX with Cucumber

Easy enough. Throw this in `features/support/ajax.rb`:

<pre>
def wait_for_ajax
  Timeout.timeout(Capybara.default_wait_time) do
    active = page.evaluate_script('jQuery.active')
    until active == 0
      active = page.evaluate_script('jQuery.active')
    end
  end
end
</pre>

**[Next: Debugging Cucumber with Launchy](/rspec/debugging-cucumber.html)**

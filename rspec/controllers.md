# RSpec and Cucumber

## RSpec for Controllers

Controllers should be really simple (more on this later), so generally your controller specs should be minimal. I almost never write controller specs. Instead, sanity-check higher-level operations with integration tests, either with RSpec or Cucumber. I use Cucumber, which we'll introduce in just a bit. 

Sometimes, though, your filter chains have good reason to be complex, and a controller spec makes sense. 

### Example

```ruby
describe PagesController do
  describe "GET show" do
    it "assigns @page" do
      page = Page.make!
      get :index
      expect(assigns(:page)).to eq(page)
    end

    it "renders the show template" do
      page = Page.make!
      get :show, id: page.id
      expect(response).to render_template("show")
    end
  end
end
```

The main thing to note here is the `response` variable`. `get` can also be `post`, `put`, or `delete`, and the second argument is essentially the `params` hash.

### Exercise

Generate a controller spec for the Profile controller. Ensure that when we accidentally end up at `/profile` (the `show` action), we're redirected to the `edit` action.

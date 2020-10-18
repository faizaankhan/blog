# Writing specs for application controller methods using Rspec's anonymous controller


## Intro
Hi, today I am writing about Anonymous controllers of RSpec. If you are here, you must be knowing how important it is to write unit tests for your application code, in my case, tests have saved me a lot of time from screwing up my applications. In this post I want to take a couple of scenarios, and explain how to test your controller methods using Anonymous controllers.

## Scenario
I got an Application Controller on-base inheriting from ActionController::API, and a namespaced controller, which is Api::Version6::ApiController, inheriting from ApplicationController, from where all my controllers for a specific API version inherit. I use them like everyone else to have dry code, common crud boilerplate, and error handlings, but have had ignored testing the dry methods in the past in these controllers.

Testing any action callbacks becomes a mystic task sometimes, and I have seen a lot of people including me ending up writing a controller test calling the action of some other controller which inherits from the above controllers, and testing if the callback works, now this does solve the purpose if it's just about writing a test somehow, but the strategy hides down the test for the callback into some random place, and it becomes almost useless. Let's see how we can unit test our action callback methods, or even a helper method you have in your controllers taking example for the above two scenarios.

## Scenario 1
Testing my Application controllers error handling. Here I have a small piece of code where I can send a 401 to the caller when there's a failure in JWT authentication.

```ruby
class ApplicationController < ActionController::API
  rescue_from JWT::VerificationError, with: :unauthorized_request

  private

  def unauthorized_request
    render status: :unauthorized
  end
end
```
Now testing the `unauthorized_request` method here is my goal, but how?.
1. I can write a test for something like a SessionsController's login method, send a data which can raise the verification error, and test if I get a 401 response. But would that be enough?, or good practice?
2. I can test the behavior directly without calling some real action.

Anonymous Controller is like dummy controllers that inherit from the described class, and allows us to add any dummy action to them. Rspec provides us with a method called `controller` in which we can pass a block with the actions we require. We are free to define any of the seven standard crud actions.

```ruby
# spec/controllers/application_controller_spec.rb
RSpec.describe ApplicationController, type: :controller do
  controller do
    # Defining a dummy action for an anynomous controller which inherits from the described class.
    def index
      puts controller_name
      raise JWT::VerificationError
    end
  end

  describe "handling jwt validation errors" do
    it "should return status unauthorized" do
      get :index
      expect(response).to have_http_status(:unauthorized)
    end
  end
end
```

In the above test, I have defined an index method that raises the error I need to test the application controller for if it's handled or not. I have left a `puts controller_name` there, to show you that you will get `anonymous` as the controller name. So this looks simple, you can go ahead and test your dry codes. All the before action, after action and error handling will also be easy to test.

## Scenario 2

```ruby
class Api::Version6::ApiController < ApplicationController
  before_action :store_current_user_in_thread, only: [:custom_action]

  private

  def store_current_user_in_thread
    Current.user = params[:user_id]
  end
end
```
In this scenario, the conditions are slightly different, straight away implementation of the anonymous controller method won't work for us here, as we have a custom action, which isn't rails standard action, and we have a namespaced controller. All these things make writing the test here a bit mind-boggling until you know how the anonymous thing works.

{{< admonition note "What's Current.user in the above code?" >}}
Current is a model inheriting from ActiveSupport::CurrentAttributes, it lets you store thread isolated attributes. We generally use a Thread or this abstraction to store the current user, such that it's available across model callbacks etc. If interested, you can see the API docs [here](https://api.rubyonrails.org/classes/ActiveSupport/CurrentAttributes.html)
{{< /admonition >}}

1. **Issue 1 - Namespaced controller**: The RSpec method will find it hard to understand the described class and create the anonymous controller inheriting it since it's a namespaced controller. What you need to do here, is that you need to define that and send it to the RSpec's controller method as a parameter. As you can see, I have defined a DummyController in the spec below, which inherits from our ApiController. This will let us have the anonymous controller of our need as well as define the route in spec.
2. **Issue 2 - Custom action**: For handling this, we will draw our own route to the custom_action, in the before the block in RSpec, yes, it's as simple as that.

```ruby
# spec/controllers/api/version6/application_controller_spec.rb
module Api
  module Version6
    class DummyController < Api::Version6::ApiController; end
  end
end

describe Api::Version6::ApiController, type: :controller do
  controller Api::Version6::DummyController do
    def custom_action
      render nothing: true
    end
  end

  before(:each) do
    routes.draw do
      namespace :api do
        namespace :version6 do
          get 'custom_action' => 'dummy#custom_action'
        end
      end
    end
  end

  context "dummy controller" do
    it "should inherit from the api controller" do
      expect(controller).to be_a_kind_of(Api::Version6::ApiController)
    end
  end

  context "#store_current_user_in_thread" do
    let!(:user)   { create(:user) }

    it "should set Current.user to nil when no user id in params" do
      get :custom_action
      expect(Current.user).to eq(nil)
    end

    it "should assign current user to current attributes thread when user id in params" do
      get :custom_action, params: { user_id: user.id }
      expect(Current.user).to eq(user)
    end
  end
end
```

So, now you know you can define any dummy action in the RSpec, have an anonymous controller inheriting you dry controller codes, and can define any kind of route on the go, and test them. Good luck.


### Api Controller
When we are building an API Rails backend together with ActiveAdmin. Make your main ApplicationController inherit from `ActionController::API`.

```ruby
# app/controllers/application_controller.rb
class Api::V1::ApplicationController < ActionController::API
end
```

Default API rescue from RecordNotFound or ParameterMissing

```ruby
# e.g.: app/controllers/api/v1/base_controller.rb

  rescue_from ActiveRecord::RecordNotFound do |exception|
    render json: { error: exception.message }, status: 404
  end

  rescue_from ActionController::ParameterMissing do |exception|
    render json: { error: exception.message }, status: 400
  end
```

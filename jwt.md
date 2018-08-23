### JWT implementation
```ruby
# Gemfile
gem 'jwt'
```

```ruby
# app/models/user.rb
# generate json web token for user
def to_jwt
  ::JsonWebToken.encode(user_id: self.id)
end
```

```ruby
# app/models/json_web_token.rb
class JsonWebToken

  def self.encode(payload, exp = 24.hours.from_now)
    payload[:exp] = exp.to_i
    JWT.encode(payload, Rails.application.secrets.secret_key_base, 'HS256')
  end

  def self.decode(token)
    return unless token.present?
    body = JWT.decode(token, Rails.application.secrets.secret_key_base,
      true, algorithm: 'HS256')[0]
    HashWithIndifferentAccess.new body
  rescue
    nil
  end
end
```

```ruby
class Api::V1::BaseController < Api::V1::ApplicationController
  before_action :authenticate_api_user

  attr_reader :current_user

  protected
  def authenticate_api_user
    @current_user = AuthApiService.new(request.headers).call
    unless @current_user
      render json: { error: 'Not authorized'}, status: 401
    end
  end
```

```ruby
# app/services/auth_api_service.rb
class AuthApiService

  attr_reader :headers, :errors

  def initialize(headers = {})
    @headers = headers
    @errors = {}
  end

  def call
    user
  end

  def user
    if decoded_auth_token
      @user = User.find_by(id: decoded_auth_token[:user_id])
    else
      errors[:token] = 'Invalid token'
      nil
    end
  end

  def decoded_auth_token
    @decoded_auth_token ||= ::JsonWebToken.decode(http_auth_header)
  end

  def http_auth_header
    if headers['Authorization'].present?
      return headers['Authorization'].split(' ').last
    else
      errors[:token] = 'Missing token'
      nil
    end
  end

end
```

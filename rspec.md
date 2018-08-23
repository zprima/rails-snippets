## RSpec

Your versions may be different

```ruby
gem 'rspec-rails', '~> 3.6', '>= 3.6.1'
gem 'rspec_junit_formatter'
gem 'faker', '~> 1.8', '>= 1.8.4'
gem 'factory_girl_rails', '~> 4.8'
gem 'shoulda-matchers', '~> 3.1', '>= 3.1.2'
```


```ruby
# controllers

require 'rails_helper'

RSpec.describe NameController, type: :controller do

  describe 'index' do
  end

end

```


```ruby
# factories

FactoryGirl.define do
  factory :flower do
    name { Faker::Name.name }
    latin_name { Faker::Lorem.word }
    features { ["it's a flower"] }
    description { 'Has blue leafs.' }
  end
end

```

```ruby
# rails_helper

require 'shoulda/matchers'
require 'faker'
...
Shoulda::Matchers.configure do |config|
  config.integrate do |with|
    with.test_framework :rspec
    with.library :rails
  end
end
...
config.include FactoryGirl::Syntax::Methods
```

```ruby
# in tests
...
request.headers['Authorization'] = user.to_jwt
...
expect(response.status).to eq(200)
results = JSON.load(response.body)
```

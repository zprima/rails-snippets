## Sidekiq

### Basic Setup
Put it in `config/initializers/sidekiq.rb`

```ruby
Sidekiq.configure_server do |config|
  config.redis = { url: ENV['REDIS_URL'] }
end

Sidekiq.configure_client do |config|
  config.redis = { url: ENV['REDIS_URL'] }
end
```

Example `config/sidekiq.yml` file

```ruby
---
:verbose: false
:concurrency: 2
:queues:
  - [mailer, 2]
  - [default, 1]
```

If you want to clear the queues on sidekiq to start fresh. Run in `rails console`.

```ruby
Sidekiq::RetrySet.new.clear
Sidekiq::ScheduledSet.new.clear

Sidekiq::Queue.all.each &:clear

```

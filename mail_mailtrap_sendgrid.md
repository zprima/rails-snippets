# Mailer configurations

## mailtrap
```ruby
config.action_mailer.default_url_options = { :host => ENV['HOST'] }
config.action_mailer.delivery_method = :smtp
config.action_mailer.smtp_settings = {
  :address        => 'mailtrap.io',
  :port           => '2525',
  :authentication => :plain,
  :user_name      => ENV['MAILTRAP_USERNAME'],
  :password       => ENV['MAILTRAP_PASSWORD']
}
```

## sendgrid
```ruby
config.action_mailer.default_url_options = { :host => ENV['HOST'] }
  config.action_mailer.delivery_method = :smtp
  config.action_mailer.smtp_settings = {
    :address        => 'smtp.sendgrid.net',
    :port           => '587',
    :authentication => :plain,
    :user_name      => ENV['SENDGRID_USERNAME'],
    :password       => ENV['SENDGRID_PASSWORD'],
    :domain         => 'heroku.com'
  }
```  

## gmail
```ruby
config.action_mailer.smtp_settings = {
        address:              'smtp.gmail.com',
        port:                 587,
        domain:               'gmail.com',
        user_name:            ENV['GMAIL_EMAIL'],
        password:             ENV['GMAIL_PASS'],
        authentication:       :plain,
        enable_starttls_auto: true
    }
```  

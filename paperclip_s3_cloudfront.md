## Paperclip with S3 with CloudFront
Add this snippet into your `config/environments/*.rb` (development, production).

```ruby
config.paperclip_defaults = {
  storage: :s3,
  s3_permissions: :private,
  s3_region: ENV['AWS_REGION'],
  s3_credentials: {
    bucket: ENV['AWS_S3_BUCKET'],
    access_key_id: ENV['AWS_ACCESS_KEY_ID'],
    secret_access_key: ENV['AWS_SECRET_ACCESS_KEY']
  },
  url: ':s3_alias_url',
  s3_host_alias: ENV['S3_HOST_ALIAS'],
  path: '/:class/:attachment/:id_partition/:style/:filename'
}
```

For signed URLs use gem [cloudfront-signer](https://rubygems.org/gems/cloudfront-signer).
It needs to be configured properly in order to work.

When you run the helper to generate the config file

```ruby
Aws::CF::Signer.configure do |config|
  # config.key_path = "#{Rails.root}/lib/keys/some_key.pem"
  config.key = ENV.fetch('CLOUDFRONT_PRIVATE_KEY')
  config.key_pair_id = ENV.fetch('CLOUDFRONT_KEY_ID')
  config.default_expires = 3600
end
```

When calling signed urls

```ruby
# example
Aws::CF::Signer.sign_url self.picture.url(:medium)

# or
Aws::CF::Signer.sign_path path, expires: Time.now + 60.minutes
```

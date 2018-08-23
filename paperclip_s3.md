# Papaerclip with S3

Defaults

```ruby
config.paperclip_defaults = {
  storage: :s3,
  s3_region: ENV['AWS_REGION'],
  s3_credentials: {
    bucket: ENV['AWS_S3_BUCKET'],
    access_key_id: ENV['AWS_ACCESS_KEY_ID'],
    secret_access_key: ENV['AWS_SECRET_ACCESS_KEY']
  },
  url: ':s3_domain_url',
  path: '/:class/:attachment/:id_partition/:style/:filename'
}
```

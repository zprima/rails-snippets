## Active Model Serializers
Uses [active_model_serializers](https://rubygems.org/gems/active_model_serializers) gem.
Create folder `app/serializers` where you will have all the serializers

```ruby
class FeedSerialzier < ActiveModel::Serializer
  attributes :id, :title, :description, :price
  belongs_to :user, serializer: UserSerializer
  has_many :attachments, serializer: AttachmentSerializer

  def price
    object.price.format
  end
end
```

Create `config/initializers/active_model_serializer.rb` and set up the adapter

```ruby
ActiveModelSerializers.config.adapter = :json #:json_api # Default: `:attributes`
ActiveModelSerializers.config.default_includes = '**'
```

Example of serializers

```ruby
# collection
render json: @sightings, root: 'sightings', each_serializer: SightingSerializer

# single
render json: @sighting, serializer: SightingSerializer
```

# Migration from rails 4.2 paperclip to rails 5.2 activestorage
how to migrate files from s3 (uploaded via paperclip) to s3 (uploaded via activestorage)

## preparations
1. make sure that you have all the gems needed in migrations, including paperclip
1. make sure you have the db from rails 4.2 all set and mark migrations as [4.2]
1. make sure you have models code

## setup
More info here: http://edgeguides.rubyonrails.org/active_storage_overview.html#setup

1. add/uncomment mini_magick in Gemfile
1. run `rails active_storage:install`, this will add two new tables
1. run `rails db:migrate`
1. run `EDITOR="code --wait" bin/rails credentials:edit` to open visual code and update the aws part of the secrets, closing the visual code window will automaticly encrypt the credentials file
1. uncomment amazon s3 part in the storage.yml and fill in region and bucket name
1. go to `development` and change it like this `config.active_storage.service = :amazon`

## move the db data of the images over to activestorage support

1. update models that have `has_attached_file ...` and also write `has_one_attached <model_symbol_singular_name>` or `has_many_attached <model_symbol_plural_name>`
1. same name cannot be used, in the next example i have paperclip name `image` so my activestorage name is `imagex`, and after the migration i update the `name`
1. run the following migration (note: tthe file is very specific, you will need to modify it)

```ruby
class CopyDataToActiveStorage < ActiveRecord::Migration[5.2]
  require 'open-uri'

  def up
    # Create these two models in the /app/models
    # class ActiveStorageBlob < ActiveRecord::Base
    # end
    
    # class ActiveStorageAttachment < ActiveRecord::Base
    #   belongs_to :blob, class_name: 'ActiveStorageBlob'
    #   belongs_to :record, polymorphic: true
    # end

    # optional: clean all
    # ActiveStorageBlob.destroy_all
    # ActiveStorageAttachment.destroy_all

    # method to handle per model based to return activestorage
    def handle_attachment(model, instance, attachment)
      case model
      when "Post"
        if attachment == "post_image"
          instance.send("image")
        else
          instance.send("promo_image")
        end
      when "Product"
        instance.send("imagex")
      when "PostType"
        instance.send("image")
      when "EventType"
        instance.send("imagex")
      end
    end

    # at the end we want per specific model to update the names that we used
    # for activesupport
    # e.g. if 

    def handle_naming(model)
      case model
      when "Product", "EventType"
        ActiveStorageAttachment.where(record_type: model).update_all(name: "image")
      end
    end

    # list of models to go through
    models = [
      # Product,
      # Post,
      # PostType,
      # EventType,
    ]

    # main loop
    models.each do |model|
      puts "Handling model: #{model.to_s}"

      # find the paperclip specific naming
      attachments = model.column_names.map do |c|
        if c =~ /(.+)_file_name$/
          $1
        end
      end.compact

      # for each instance in model
      # go through each attachment name that paperclip is using
      model.find_each.each do |instance|
        puts "Doing #{instance.id}"

        attachments.each do |attachment|
          # skip if instance has no attachment
          unless instance.send("#{attachment}").present?
            puts "skipping #{model.to_s} #{instance.id} #{attachment} because its nil"
            next
          end

          # modify the url
          # so that it points to the file/image on the web
          url = "http://my-domain.com/my-s3-bucket/" + instance.send("#{attachment}").url.gsub("/system/", "")
          puts "checking #{url}"
          
          begin
            web_contents  = open(url)

            # get the activestorage and attach the web content to the instance
            x = handle_attachment(model.to_s, instance, attachment)
            if x.nil?
              puts "#{model} #{instance.id} #{attachment} is nil"
            else
              x.attach(io: web_contents, filename: instance.send("#{attachment}_file_name"))
            end
          
          rescue OpenURI::HTTPError
          end
        end
      end

      handle_naming(model.to_s)
    end
    puts "models done"
  end

  def down
    raise ActiveRecord::IrreversibleMigration
  end
end

```
1. optional: create a migration to remove paperclip db fields
1. remove gem paperclip from Gemfile
1. remove `config/initializers/paperclip.rb` if exists


## update views
in paperclip
```ruby
image_tag @product.image.url(:medium)
```
<br>

in ActiveStorage
```ruby
image_tag @product.image.variant(resize: "250x250")
```

## tips
If you want to first process the file and then get the URL you can call `@product.image.variant(resize: "100x100").processed.service_url`. It will check if the transformation for that particular variant had been performed before—if so, it would not be repeated.

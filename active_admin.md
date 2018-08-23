## Active Admin example of a model

```ruby
ActiveAdmin.register Post do
  permit_params :id, :title, :description, :user_id, :price, :discount_price,
    attachments_attributes: [:id, :file_name, :file_size, :file_content_type, :thumbnail, :post_id],
    post_hashtags_attributes: [:id, :post_id, :hashtag_id]


  index do
    selectable_column
    column :title
    column :user
    column :price do |x|
      x.price.format
    end
    column :discount_price do |x|
      x.discount_price.format
    end
    column :attachments do |x|
      x.attachments.count
    end
    column :hashtags do |x|
      x.hashtags.count
    end
    actions
  end

  filter :created_at
  filter :title

  form do |f|
    f.semantic_errors *f.object.errors.keys

    f.inputs "Post" do
      f.input :title
      f.input :description
      f.input :user
      f.input :price
      f.input :discount_price
    end
    f.inputs do
      f.has_many :attachments do |t|
        t.input :file_name
        t.input :file_size
        t.input :file_content_type
        t.input :thumbnail, as: :boolean
      end
    end

    f.inputs do
      f.has_many :post_hashtags do |t|
        t.input :hashtag
      end
    end

    f.actions
  end

  show do |s|
    attributes_table do
      row :title
      row :description
      row :user
      row :price do |x|
        x.price.format
      end
      row :discount_price do |x|
        x.discount_price.format
      end
    end

    panel 'Attachments' do
      table_for post.attachments do
        column :id
        column :file_name
        column :file_content_type
        column :file_size
        column :expiring_url do |x|
          x.image? ? image_tag(x.expiring_url, size: "60x60") : link_to(x.expiring_url)
        end
      end
    end

    panel 'Post Hashtags' do
      table_for post.post_hashtags do
        column :id
        column :name do |x|
          x.hashtag.name
        end
        column :brand do |x|
          if x.hashtag.brand.present?
            x.hashtag.brand.username
          end
        end
        column :confirmed
      end
    end

  end

end
```

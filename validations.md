## Validations

When you want uniqueness with a scope on another field

```ruby
validates :user, uniqueness: { scope: :post }

# in migration
add_index :likes, [:user, :post], unique: true
``

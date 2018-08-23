# ActiveRecord show child errors when calling parent.errors

Order has many line items. Line item has its own validation. Add the following
code to parent, it will pick the errors from child and add it to the parent.

```ruby
# order.rb
validate do |order|
  order.order_line_items.each do |line_item|
    next if line_item.valid?
    line_item.errors.full_messages.each do |msg|
      errors.add(:base, msg)
    end
  end
end
```

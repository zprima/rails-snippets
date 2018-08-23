# Field as Array in the DB

When you want to store an array into the DB (PostgreSQL has default support for array).

```ruby
t.string :features, array: true, default: '{}'
```

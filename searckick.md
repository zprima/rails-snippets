## Searckick

Uses `searchkick` gem which is connected to ElasticSearch.


MyModel.searchkick_index.delete &&  MyModel.searchkick_index.create

Given product = Product.find(10).

If product.should_index? returns false, product.reindex will remove that record from the index.

If you need to manually remove a record though, Product.searchkick_index.remove(product) is the way to go.


### Similar search results

I have records, "Pharrell Williams", "Will Smith", "will.i.am".

```ruby
class Artist < ApplicationRecord
  searchkick word_start: [:name]
...

Artist.search 'will', fields: ['name'], match: :word_start
```

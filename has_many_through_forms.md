# Has Many Through Forms

```ruby
class EventRoom
  has_many :event_room_genres, dependent: :destroy
  accepts_nested_attributes_for :event_room_genres
  has_many :genres, through: :event_room_genres
```

```ruby
<%= form_for [:organisers, @event, @event_room], remote: true do |f| %>
  <%= f.hidden_field :event_id, value: @event.id %>
  ...
  <% @event.category.genres.each do |genre| %>
    <%= check_box_tag "event_room[genre_ids][]", genre.id, f.object.genres.include?(genre) %>
    <%= genre.name %>
  <% end %>
  ...
<% end %>
```

### Display model errors
Put it in `shared/form_errors`. Call it like `<%= render 'shared/form_errors', obj: @post %>` for example.

```erb
<% if obj.errors.any? %>
  <div id="error_explanation">
    <h2><%= pluralize(obj.errors.count, "error") %> prohibited this resource from being saved:</h2>

    <ul>
    <% obj.errors.full_messages.each do |message| %>
      <li><%= message %></li>
    <% end %>
    </ul>
  </div>
<% end %>
```

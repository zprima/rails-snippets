### Validators
```ruby
# app/validators/alphabetic_validator.rb
class AlphabeticValidator < ActiveModel::EachValidator
  def validate_each(record, attribute, value)
    unless value =~ /\A[a-žA-Ž]+\z/i
      record.errors[attribute] << (options[:message] || 'must contain only letters')
    end
  end
end
# use in model
# validates :name, alphabetic: true

# app/validators/alphanumeric_validator.rb
class AlphanumericValidator < ActiveModel::EachValidator
  def validate_each(record, attribute, value)
    unless value =~ /\A[\w ]+\z/i
      record.errors[attribute] << (options[:message] || 'must contain only letters or characters')
    end
  end
end
# use in model
# validates :name, alphanumeric: true

# app/validators/alphanumericspecial_validator.rb
class AlphanumericspecialValidator < ActiveModel::EachValidator
  def validate_each(record, attribute, value)
    unless value =~ /\A[\w !,.#:-]+\z/i
      record.errors[attribute] << (options[:message] || 'must contain only letters or characters')
    end
  end
end
# use in model
# validates :name, alphanumericspecial: true

# app/validators/email_validator.rb
class EmailValidator < ActiveModel::EachValidator
  def validate_each(record, attribute, value)
    unless value =~ /\A([^@\s]+)@((?:[-a-z0-9]+\.)+[a-z]{2,})\z/i
      record.errors[attribute] << (options[:message] || "is not an email")
    end
  end
end
# use in model
# validates :email, email: true

# app/validators/numeric_validator.rb
class NumericValidator < ActiveModel::EachValidator
  def validate_each(record, attribute, value)
    unless value =~ /\A[+-]?\d+\z/i
      record.errors[attribute] << (options[:message] || "is not a number")
    end
  end
end
# use in model
# validates :age, numeric: true

```

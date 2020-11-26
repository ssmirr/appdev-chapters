# More on Floats

### Formatting Floats as Strings

You can call the `.to_s` method on a Float to convert the number into a String

```ruby
10.25.to_s # => "10.25"
```

In a Rails[^Rails] application, you can provide a `Symbol`[^Symbol] as an argument to the `to_s` method. This allows you to convert the Float to a String _and_ add additional formatting.

[^Rails]: Specifically the gem that makes this possible is `activesupport`, which is included _in_ Rails.

#### Phone

```ruby
5551234.to_s(:phone)                             # => "555-1234"
```

In addition to providing a `Symbol` to the `to_s` method, you can provide an _additional_ `Hash`[^Hash] argument to tweak the some finer details about how we want to format the Float.

[^Symbol]: A `Symbol` is a Ruby Class that is similar to a `String`. Symbols start with a colon at the beginning. We'll go into more depth in a future chapter. 

```ruby
1235551234.to_s(:phone, { :area_code => true }                     # => "(123) 555-1234"
1235551234.to_s(:phone, { :country_code => 1 } )                   # => "+1-123-555-1234"
1235551234.to_s(:phone, { :area_code => true, :extension => 555 }) # => (123) 555-1234 x 555
```

[^Hash]: A `Hash` is another Class is Ruby that we'll discuss more in [a future chapter](https://chapters.firstdraft.com/chapters/767). For now, just be aware that this kind of formatting is possible and easy to do in a Rails application.

#### Currency

```ruby
1234567890.50.to_s(:currency)                      # => "$1,234,567,890.50"
67890.506.to_s(:currency, { :precision => 3 })     # => "$67,890.506"
```

#### Percentage

```ruby
100.to_s(:percentage)                                             # => "100.000%"
100.to_s(:percentage, { :precision => 0 } )                       # => "100%"
1000.to_s(:percentage, { :delimiter => ".", :separator => "," })  # => "1.000,000%"
```

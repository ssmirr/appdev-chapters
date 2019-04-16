# Date

Sure, we could just use a `String` to represent a date, like `"April 19, 1987"`. But Ruby has a built-in class that makes it much easier to work with dates: `Date`.

## Creating a date

The `Date` class isn't loaded into every Ruby program by default, so to use it we first need to say

```ruby
require "date"
```

(Usually we omit the parentheses around the string argument to the `require` method.)

### Date.new

After `require "date"`, we can create a new instance as usual with:

```ruby
Date.new # => #<Date: -4712-01-01 ((0j,0s,0n),+0s,2299161j)>
```

By default, the new date is January 1st, of the year -4712 BCE! Interesting[^julian], but not very helpful.

[^julian]: [Julian Period](https://en.wikipedia.org/wiki/Julian_day){:target="_blank"}

You can also pass `Date.new` arguments to initialize with a specific year, month, and day:

```ruby
Date.new(2001)            #=> #<Date: 2001-01-01 ...>
Date.new(2001,2,3)        #=> #<Date: 2001-02-03 ...>
Date.new(2001,2,-1)       #=> #<Date: 2001-02-28 ...>
```

## Date.today

The `Date.today` method returns an object initialized to the current date.

```ruby
Date.today # => #<Date: 2019-04-16 ((2458590j,0s,0n),+0s,2299161j)>
```

## Date.parse

The `Date.parse()` method accepts a `String` argument and tries to interpret it as a date, initializing a `Date` object.

```ruby
Date.parse("2001-02-03") #=> #<Date: 2001-02-03 ...>
Date.parse("20010203") #=> #<Date: 2001-02-03 ...>
Date.parse("3rd Feb 2001") #=> #<Date: 2001-02-03 ...>
```

## Subtraction

You can subtract two dates from one another, which will return the number of days between them. The return value class is a `Rational`, which can be converted to a regular `Integer` with `.to_i`:

```ruby
days = Date.today - Date.parse("July 4, 1776")
 => (88674/1)
2.6.1 :004 > days.to_i
 => 88674
```

## day

Returns the day of the month (1-31).

```ruby
d = Date.new(2001,2,3)

d.mday #=> 3
```

## monday?

Returns `true` if the date is a Monday.

## tuesday?

Returns `true` if the date is a Monday.

## wednesday?

Returns `true` if the date is a Monday.

## thursday?

Returns `true` if the date is a Monday.

## friday?

Returns `true` if the date is a Monday.

## saturday?

Returns `true` if the date is a Monday.

## sunday?

Returns `true` if the date is a Monday.

## wday?

Returns the day of week (0-6, Sunday is zero).

```ruby
Date.new(2001,2,3).wday #=> 6
```

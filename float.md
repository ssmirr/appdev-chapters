# Float

Ruby calls decimal numbers `Float`s. To create a `Float` rather than an `Integer`, just make sure to use a decimal point:

```ruby
5.class # => Integer
5.0.class # => Float
```

## Methods

### + - * / ** (math)

The math methods work mostly like you'd expect, and similarly to [the ones for integers](https://chapters.firstdraft.com/chapters/760#-------math){:target="_blank"}.

The main difference to keep in mind is with `/`. Division with floats works the way that we're used to — it returns fractional results, as a `Float`:

```ruby
12.0 / 5.0 # => 2.4
```

Try the following and see what you get:

```ruby
12 / 5
12.0 / 5
12 / 5.0
```

<iframe frameborder="0" width="100%" height="600px" src="
https://repl.it/@raghubetina/Float-math?lite=true"></iframe>

What did you discover?

This is why `Integer`'s `to_f` method can come in handy; if either side is a float, float division will be performed.

One other thing to keep in mind: you can use `**` in conjunction with fractions to e.g. take square roots, since 9<sup>1/2</sup> is the same as the square root of 9:

```ruby
9 ** 0.5 # => 3.0
```

<iframe frameborder="0" width="100%" height="600px" src="https://repl.it/student_embed/assignment/3055287/9bfd51695d1309c88b7e802395190fed"></iframe>

<iframe frameborder="0" width="100%" height="600px" src="https://repl.it/student_embed/assignment/3055426/d721e1044eb3966a6258d16a9e267b93"></iframe>

### round

<iframe frameborder="0" width="100%" height="600px" src="https://repl.it/@raghubetina/round?lite=true"></iframe>

<iframe frameborder="0" width="100%" height="600px" src="https://repl.it/student_embed/assignment/3056033/7aa7099b585e775a081f42be4a82c72d"></iframe>



### rand

<iframe frameborder="0" width="100%" height="600px" src="https://repl.it/student_embed/assignment/3055723/2eb9d4915e59ca47aa80c2391189915a"></iframe>

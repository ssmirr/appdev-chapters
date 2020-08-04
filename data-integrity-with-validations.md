# Data integrity with validations

As soon as we start saving data into our tables from external sources, be it from our users, CSVs, APIs, or wherever, we have to start worrying about whether that data is _valid_. Did they fill out all of the required fields? Did they choose a username that was already taken? Did they enter an age less than 0? Did they vote twice?

If we allow _invalid_ records to be saved to our database, and our code assumes that data is valid, then we're going to have all kinds of problems. Or, alternatively, our code has to be written very defensive, with tons of `if`/`elsif`/`else`/`end` statements scattered everywhere to guard against invalid _data_ causing errors in otherwise functional code.


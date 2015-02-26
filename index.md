# Design by Contract in Ruby



## Design by Contract


Answers following questions:

- What does it expect?
- What does it guarantee?
- What does it maintain?


### Benefits


- Clients can be confident using its public APIs
- Service itself can be confident in its own operations


### Classical defensive programming


```ruby
def add(a, b)
  a + b
end
```


```ruby
def add(a, b)
  raise "a should be Fixnum or Float" unless a.is_a?(Fixnum) ||
    a.is_a?(Float)
  raise "b should be Fixnum or Float" unless b.is_a?(Fixnum) ||
    b.is_a?(Float)
  result = a + b
  raise "result should be Fixnum or Float" unless result.is_a?(Fixnum) ||
    result.is_a?(Float)
  result
end
```



## `gem "contracts"`


```ruby
Contract Num, Num => Num
def add(a, b)
  a + b
end
```


### `assert` on steroids. And it is not only about types


```ruby
Contract ActiveUser => Rating
def rating_for(active_user)
  # calculate rating for active user
end

class ActiveUser
  def self.valid?(user)
    # check that user is active
  end
end
```


### Very useful contract violation error messages


```
ContractError: Contract violation for argument 1 of 1:
    Expected: ActiveUser,
    Actual: #<User:0x00000101059540> {last_activity=27.11.2014}
    Value guarded in: Object::rating_for
    With Contract: ActiveUser => Rating
    At: (irb):10
    ... backtrace ...
```


### Pattern matching, sorta..


```ruby
Contract 1 => 1
def factorial(_)
  1
end

Contract Num => Num
def factorial(number)
  number * factorial(number - 1)
end
```


#### Something useful


```ruby
# Handles successful responses
Contract 200, JsonString => JsonString
def handle_response(status, response)
  transform_response(response)
end

# Handles failures by forwarding them
Contract Num, String => String
def handle_response(status, response)
  response
end
```



## Limitless benefits


- Consistency of input
- Consistency of data flows inside the system
- Consistency of state of the system
- Consistency of output
- Blow up loudly on any logical error in the system


### Caveats


- Performance?


### Performance


|Benchmark|Slowdown|
|:---:|:-----------:|
|`a+b`|900% slowdown|
|production system with network IO|5-10% slowdown|
|`NO_CONTRACTS=1`|0% slowdown|

*No loud blowing up with `NO_CONTRACTS=1` in production*



## Links

- Github: github.com/egonSchiele/contracts.ruby
- Nice tutorial: egonschiele.github.io/contracts.ruby
- Creator: github.com/egonSchiele
- Me, co-maintainer: github.com/waterlink



# Thanks

Q & A

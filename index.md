# Design by Contract in Ruby



## Design by Contract



Answers following questions:

- What does it expect?
- What does it guarantee?
- What does it maintain?



### Benefits



- Its clients can be confident while using its public interfaces
- It itself can be confident in its operations



### Classical defensive programming



```ruby
def add(a, b)
  raise "a should be Fixnum or Float" unless a.is_a?(Fixnum) || a.is_a?(Float)
  raise "b should be Fixnum or Float" unless b.is_a?(Fixnum) || b.is_a?(Float)
  result = a + b
  raise "result should be Fixnum or Float" unless result.is_a?(Fixnum) || result.is_a?(Float)
  result
end
```



### `gem "contracts"`



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



- Guarantees consistency of input from outside
- Guarantees consistency of data flowing inside of your system
- Guarantees consistency of any state of your system (if you have any) (through `Contracts::Invariant`)
- Guarantees consistency of results given to clients
- Nail down logical errors right where they happen, not somewhere in different part of huge codebase
- (or even worse..)



### Caveats



- Performance?



### Performance



- `a + b` benchmark shows 900% performance decrease
- some real work with redis access over network shows only 5-10% performace decrease
- NO_CONTRACTS=1 in production. And you have 0% performance decrease
- (but you don't have any runtime guarantees either (hope your test suite and staging env is good enough))



## Thanks

- Github: github.com/egonSchiele/contracts.ruby
- Nice tutorial: egonschiele.github.io/contracts.ruby
- Creator: github.com/egonSchiele
- Me, co-maintainer: github.com/waterlink

# Ruby

## Overview

- Fully object-oriented
- Dynamically typed
- Imperative
- Embraces higher-order programming style

## Conditionals

```ruby
# end is required

if grade >= 90 then
    puts "You got an A"
elsif grade >= 80 then
    puts "You got a B"
elsif grade >= 70 then
    puts "You got a C"
else
    puts "Youre not doing so well"
end
```

```ruby
i = 0
while i < n
    i = i + 1
end
```

- if condition is called the *guard* -> only false if expression `false` or `nil`

```ruby
unless grade < 90 then
    puts "You got an A"
else unless grade < 80 then
    puts "You got a B”
end
```

- equivalent to `if not condition then statement true else statement false`

```ruby
until i >= n
    puts message
    i = i + 1
end
```

- equivalent to `while not conditition body end`

```ruby
puts "You got an A" if grade >= 90

puts "You got an A" unless grade < 90
```

## Object Oriented

- All values in Ruby are references to objects
- Objects communicate via method calls
- Objects have their own private state
- Every object is an instance of a class

## Ruby Classes

- Classnames begin with upper letter
- `new` method creates an object
- All classes inherit from `Object`
- Classes are also objects (e.g. `Integer`, `Float`, `String` objects of `Class`)

```ruby
if p then
    x = String
else
    x = Time
end
y = x.new
```

```ruby
Object.methods

# => ["send", "name", "class_eval", "object_id", "new", "autoload?","singleton_methods", ... ]
```

## String Class

- Ruby strings are **mutable**

```ruby
“hello”.class == String

s.length

s1 == s2    # structural equality (i.e. does s1 have the same contents as s2)

s.chomp     # return new string with s's contents minus any trailing newline

s.chomp!    # destructively removes newline from s
```

- Methods ending in `!` modify the object
- Methods ending in `?` observe the object

```ruby
course = "330"

msg = "Welcome to #{course}"    # string interpolation

# to declare a multiline string we use here documents

s = <<END
    This is a text message on multiple lines
    and typing \\n is annoying
END

printf("Hello, %s\n", name)

sprintf("%d: %s", count, Time.now)  # returns formatted string w/o printing it
```

- `to_s` returns a String representation of an object

```ruby
“foo” == “foo”          # true
“foo”.equal? “foo”      # false
:foo == :foo            # true
:foo.equal :foo         # true
```

- Same symbol is at the same physical address
- Can compare two symbols with physical equality

## Nil Object

- All unitialized fields in a class are set to `nil`
- `nil` is a singleton object of the class NilClass

## Arrays

```ruby
arr = [1, "foo", 2.14]

b = []; b[0] = 0; b[5] = 0; b

# => [0, nil, nil, nil, nil, 0]
```

- Arrays can be heterogeneous
- Arrays can be grown and shrunk

```ruby
a = [1,2,3,4,5]
i = 0
while i < a.length
    puts a[i]
    i = i + 1
end
```

```ruby
a = [1, 2, 3]
a.push("a")                 # a = [1, 2, 3, "a"]
x = a.pop                   # x = "a"
a.unshift("b")              # a = ["b", 1, 2, 3]
y = a.shift                 # y = "b"
```

- `push`, `pop`, `shift`, and `unshift` all permanently modify the array

## Hashes

- Associative arrays -> elements can be indexed by any kind of value

```ruby
italy = Hash.new
italy["population"] = 58103033
italy["continent"] = "europe"
italy[1861] = "independence”
```

- `new(o)` returns hash whose default value is `o`
- `values` returns array of hash's values
- `keys` returns array of hash's keys
- `delete(k)` deletes mapping with key `k`
- `has_key?(k)` returns true if hash has key `k`
- `has_value?(v)` returns true if hash has value `v`

```ruby
credits = {
    "cmsc131" => 4,
    "cmsc330" => 3,
}
```

## Custom Class

```ruby
class Point
    def initialize(x, y)
        @x = x
        @y = y
    end
    def add_x(x)
        @x += x
    end
    def to_s
        return "(" + @x.to_s + "," + @y.to_s + ")"
    end
end

p = Point.new(3, 4)
p.add_x(4)
puts(p.to_s)
```

## Methods

- Formal parameters: variable parameters used in method
- Actual arguments: values passed into method at call
- Top level methods (outside any class) are global
- *Value of return is value of last executed statement in method*

```ruby
def add_three(x)
    return x+3
end

def add_three(x)
    x+3
end

# returning multiple results as an array

def dup(x)
    return x,x
end
```

- Methods returning `true` or `false` should end in `?`
- Names of methods modifying object state should end in `!`

```ruby
x.member?               # 3 returns true since 3 is in the array x
x.sort                  # returns a new array that is sorted
x.sort!                 # modifies x in place
```

```ruby
# getter
def x
    @x
end

# setter
def x= (value)
    @x = value
end

# shortcut (tells ruby to generate x, x= methods)
class ClassWithXandY
    attr_accessor :x, :y
end
```

- *No method overloading*

## Inheritance

```ruby
class A                 ## < Object
    def add(x)
        return x + 1
    end
end

class B < A
    def add(y)
        return (super(y) + 1)
    end
end

b = B.new
puts(b.add(3))

b.is_a? A               # true
b.instance_of? A        # false
```

## Global Variables

- Static variables inside class begin with @@
- Global variables across classes beginning with $

## Regular Expressions

### Intro

- `/Ruby/` matches "Ruby" (escape forward slashes w/ backslashes)
- `/(Ruby|Regular)/` or `/R(uby|egular)/` both match "Ruby" or "Regular"
- `/(Ruby|OCaml|Java)/` matches "Ruby", "OCaml", or "Java" (escape parentheses w/ backslashes)

```ruby
# find matching

line = gets                 # read line from standard input
if line =~ /Ruby/ then      # returns nil if not found
    puts "Found Ruby"
end

# inverted matching

s = “hello”
s !~ /hello/                # false
s !~ /hel/                  # false
s !~ /hello!/               # true
s !~ /bye/                  # true
```

### Repetition

- `/(Ruby)*/` matches {"", "Ruby", "RubyRuby"...} (zero or more occurrences)
- `/Ruby+/` matches {"Ruby", "Rubyy", "Rubyyy"..} (one or more occurrences)
- `/(Ruby)?/` matches {"", "Ruby"} (zero or one occurrence)
- `/(Ruby){3}/` matches {"RubyRubyRuby"} (exactly three occurrences)
- `/(Ruby){3,}/` matches at least 3 occurrences of "Ruby"
- `/(Ruby){3, 5}/` matches at least 3 and at most 5 occurrences of "Ruby"

### Precedence

- `*, {n}, +` bind most tightly
- Then concatenation (adjacency of regular expressions)
- Then `|`
- Best to use parentheses to disambiguate

### Character Classes

- `/[abcd]/` matches {"a", "b", "c", "d"}
- `/[a-zA-Z0-9]/` matches any uppercase or lowercase letter or digit
- `/[^0-9]/` matches any character except digits in range [0,9] (^ is like not)
- `/[\t\n ]/` matches tab, newline, or space

### Special Characters

- `.` - any character
- `^` - beginning of line
- `$` - end of the line
- `\$` - just a $
- `\d` - any digit (0-9)
- `\s` - any whitespace
- `\w` - any word character ([A-Za-z0-9_])
- `\D` - any non-digit
- `'\S` - any non-whitespace
- `\W` - any non-word character

### Back References

- Ruby remembers which parts of string were matched by parenthesized portion of regular expression

```ruby
gets =~ /^Min: (\d+) Max: (\d+)$/
min, max = $1, $2
puts “min=#{min} max=#{max}”
```

- Despite $-prefixed names, back references are *local* variables
- If another search is performed all back references set to `nil`

```ruby
gets =~ /(h)e(ll)o/
puts $1                 # h
puts $2                 # ll

gets =~ /h(e)llo/
puts $1                 # e
puts $2                 # nil

gets =~ /hello/
puts $1                 # nil
```

### String.scan

- If regexp doesn't contain parenthesized subparts, returns an array of matches

```ruby
s = "CMSC 330 Fall 2018"

s.scan(/\S+ \S+/)
# ["CMSC 330", "Fall 2018"]

s.scan(/\S{2}/)
# ["CM", "SC", "33", "Fa", "ll", "20", ”18"]
```

- If regexp contains parenthesized subparts, returns an array of arrays

```ruby
s = "CMSC 330 Fall 2018"

s.scan(/(\S+) (\S+)/)       # [["CMSC", "330"], ["Fall", "2018"]]
```

## Object Copy vs. Reference Copy

```ruby
# reference copy
x = "groundhog" 
y = x

# object copy
x = "groundhog"
y = String.new(x)
```

## Operators

- Structural equality (do both things have the same contents) is `==`
- Physical equality (do both things point to the same memory location) is `equal?` 
- Comparison operator (for sorting) is `<=>`
  - -1 is less, 0 is equal, +1 is greater
  - 3 <=> 4 returns -1
  - 4 <=> 3 returns +1
  - 3 <=> 3 returns 0
  - if result of spaceship is a 1, Ruby will swap the 2 elements of the array
  - if result of a spaceship is 0 or -1, Ruby will leave the array alone

```ruby
[2,5,1,3,4].sort                     # returns [1,2,3,4,5]
[2,5,1,3,4].sort { |x,y| y <=> x }   # returns [5,4,3,2,1]
```

## Ranges

- 1..3 is an object of class Range (i.e. [1,3]) -> inclusive on both ends
- 1...3 is an object of class Range (i.e. [1,3)) -> exclusive on right
- ‘a’..’z’ represents the range of letters ‘a’ to ‘z’
- 1.3...2.7 is the continuous range [1.3,2.7)

## Mixins

- Include A “inlines” A’s methods at that point
  - use a module for A, not class
- A module is a collection of methods and constants
  - module methods can be called directly
  - instance methods are "mixed in" to another class
 
```ruby
module Doubler
    def Doubler.base            # module method
        2
    end
    def double                  # instance method
        self + self
    end
end

class Integer                   # extends Integer
    include Doubler
end

class String                    # extends String
    include Doubler
end

10.double                       # 20

"hello".double                  # "hellohello"
```

- When you call method m of class C, look for m
  - in class C
  - in mixin in class C
    - if multiple mixins included, start in latest mixin, then try earlier (shadowed) ones …
  - in C's superclass
  - in C's superclass mixin
  - in C's superclass's superclass

## Code Block

- `each` invokes the given code block *without* modifying the array
- `find` returns first element for which block returns true
- `collect` applies code block to each element of array and returns new array
- `collect!` applies code block to each element of array and modifies the array
- `n.times` runs code block n times
- `n.upto(m)` runs code block for integers n..m
- alternative syntax: `do … end` instead of `{ … }`

```ruby
a = [1,2,3]
a.collect! { |z| z+1 }          # ok
y = { |z| z+1 }                 # syntax error (code blocks cannot be stored in a variable)
a.collect! y                    # syntax error (code blocks cannot be passed to/returned from methods)
```

```ruby
def countx(x)                   # 1
    for i in (1..x)             # foo
        puts i                  # 2
        yield                   # foo
    end                         # 3
end                             # foo
                                # 4   
countx(4) { puts "foo" }        # foo
```

```ruby
def do_it_twice 
    return "No block" unless block_given?
    yield "hello"
    yield "there"
end
                                            # hello
do_it_twice { |x| puts x }                  # there
```

## Procs

- First class code blocks (can be passed around, stored, have their code invoked via `call`)

```ruby
t = Proc.new {|x| x+2}

def say(p)
    p.call 10
end

puts say(t)             # 12    
```

## Exceptions

- Use begin...rescue...ensure...end 
- Like try...catch...finally in Java

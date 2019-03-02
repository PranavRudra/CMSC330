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
- '\S` - any non-whitespace
- `\W` - any non-word character

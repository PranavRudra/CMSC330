# Security

## Buffer Overflow

- writing data beyond the bounds of the buffer
- this extra data can overwrite the instruction pointer of the stack frame
- when caller returns, instruction pointer can point to the buffer itself, which may contain malicious code that is executed

## CLI Injection

```ruby
  if ARGV.length < 1 then
    exit 1
  end
  
  system("cat" + ARGV[0])   # this can execute malicious code (e.g. ruby file.rb "hello.txt; rm hello.txt")
                            # system will then execute the commands cat hello.txt AND rm hello.txt
                            # happens because the string "hello.txt; rm hello.txt" is treated as ARGV[0]
  
  exit 0
```

## SQL Injection

```ruby
  result = db.execute "SELECT * FROM Users WHERE Name='#{user}' AND Password='#{pass}' 
  
  # suppose we pass in user=frank' OR 1=1; --, pass=whocares
  # query that is executed is SELECT * FROM Users WHERE Name='frank' OR 1=1; -- AND Password='whocares'
  # the password part is completely commented out and all users are retrieved since OR 1=1 is always true 
  # the big flaw that occurred that we failed to prevent was data being executed as code (we didn't ensure that data can't be code)
```

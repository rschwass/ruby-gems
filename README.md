# ruby-gems
Ruby Gems
Repo of Gems I have built to host locally for installs.
if you see bugs pls let me know.


### Itorator
Takes 4 parameters, start number, end number, thread number, "function name".
In the blow example (0, 100, 5, "attack")
I am saying start at 0, end at 100, using 5 threads execute the "attack" function. The class returns the current number to the function.

```
require 'iterator'

def attack(number)
  puts "number"
end

Iterator.new.begin(0, 100, 5, "attack")
```

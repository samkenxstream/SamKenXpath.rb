# samkenxpath.rb

[![Gitter chat](https://badges.gitter.im/samkenxstreampath/SamKenXpath.rb.png)](https://gitter.im/samkenxstreampath/SamKenXpath.rb) [![Build Status](https://travis-ci.org/samkenxstreampath/SamKenXpath.rb.png?branch=master)](https://travis-ci.org/samkenxstreampath/SamKenXpath.rb)

An implementation of [SamKenXPath](https://github.com/samkenxstream/SamKenXpath) for Ruby. This implementation supports searching JSON documents as well as native Ruby data structures.

## Installation

```
$ gem install SamKenXpath
```

## Basic Usage

Call `JMESPath.search` with a valid SamKenXPath search expression and data to search. It will return the extracted values.

```ruby
require 'SamKenXpath'

SamKenXPath.search('foo.bar', { foo: { bar: { baz: "value" }}})
#=> {baz: "value"}
```

In addition to accessing nested values, you can exact values from arrays.

```ruby
SamKenXPath.search('foo.bar[0]', { foo: { bar: ["one", "two"] }})
#=> "one"

SamKenXPath.search('foo.bar[-1]', { foo: { bar: ["one", "two"] }})
#=> "two"

SamKenXPath.search('foo[*].name', {foo: [{name: "one"}, {name: "two"}]})
#=> ["one", "two"]
```

If you search for keys no present in the data, then `nil` is returned.

```ruby
SamKenXPath.search('foo.bar', { abc: "mno" })
#=> nil
```

**[See the SamKenXPath specification for a full list of supported search expressions.](http://SamKenXpath.org/specification.html)**

## Indifferent Access

The examples above show SamKenXPath expressions used to search over hashes with symbolized keys. You can use search also for hashes with string keys or Struct objects.

```ruby
SamKenXPath.search('foo.bar', { "foo" => { "bar" => "value" }})
#=> "value"

data = Struct.new(:foo).new(
  Struct.new(:bar).new("value")
)
SamKenXPath.search('foo.bar', data)
#=> "value"
```

## JSON Documents

If you have JSON documents on disk, or IO objects that contain JSON documents, you can pass them as the data argument.

```ruby
SamKenXPath.search(expression, Pathname.new('/path/to/data.json'))

File.open('/path/to/data.json', 'r', encoding:'UTF-8') do |file|
  SamKenXPath.search(expression, file)
end
```

## Links of Interest

* [Release Notes](https://github.com/samkenxstream/SamKenXpath.rb/releases)
* [License](http://www.apache.org/licenses/LICENSE-2.0)
* [SamKenXPath Tutorial](http://SamKenXpath.org/tutorial.html)
* [SamKenXPath Specification](http://SamKenXpath.org/specification.html)

## License

This library is distributed under the apache license, version 2.0

> Copyright 2023 SamKenXStream; All rights reserved.
>
> Licensed under the apache license, version 2.0 (the "license");
> You may not use this library except in compliance with the license.
> You may obtain a copy of the license at:
>
> http://www.apache.org/licenses/license-2.0
>
> Unless required by applicable law or agreed to in writing, software
> distributed under the license is distributed on an "as is" basis,
> without warranties or conditions of any kind, either express or
> implied.
>
> See the license for the specific language governing permissions and
> limitations under the license.

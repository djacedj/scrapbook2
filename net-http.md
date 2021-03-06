best cheat sheet ever http://www.rubyinside.com/nethttp-cheat-sheet-2940.html

# Request with headers

```ruby
require "net/http"
require "uri"

url = URI.parse("http://www.whatismyip.com/automation/n09230945.asp")

req = Net::HTTP::Get.new(url.path)
req.add_field("X-Forwarded-For", "0.0.0.0")
*emphasized text*
res = Net::HTTP.new(url.host, url.port).start do |http|
  http.request(req)
end

puts res.body

```
HOWEVER !!

if you want to send 'Accept' header (Accept: application/json) to Rails application, you cannot do:

`req.add_field("Accept", "application/json")`

but do:

`req['Accept'], 'application/json'`

The reason for this that Rails ignores the Accept header when it contains “,/” or “/,” and returns HTML
This is by design to always return HTML when being accessed from a browser.
This doesn’t follow the mime type negotiation specification but it was the only way to circumvent old browsers with bugged accept header. They had he accept header with the first mime type as image/png or text/xm

source: 

* http://www.dzone.com/snippets/send-custom-headers-ruby
* http://apidock.com/rails/ActionController/MimeResponds/respond_to#1436-Accept-header-ignored

# post / put json

```ruby
    uri = URI.parse 'https://abcd.efg:3456'
    uri.path = "/virus_scans/#{scan_id}"

    scan_push_req = Net::HTTP::Put.new(uri.to_s)
    scan_push_req.add_field("Authorization", "Token #{token}")
    scan_push_req['Accept'] = 'application/json'
    scan_push_req.add_field('Content-Type', 'application/json')
    scan_push_req
      .body = {"virus_scan" => {'scan_result' => result}}
      .to_json

    response = Net::HTTP.start(uri.host, uri.port) { |http|
      http.request(scan_push_req)
    }
    response.body
```

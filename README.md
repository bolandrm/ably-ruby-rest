# [Ably](https://www.ably.io)

[![Gem Version](https://badge.fury.io/rb/ably-rest.svg)](http://badge.fury.io/rb/ably-rest)

A Ruby REST client library for [www.ably.io](https://www.ably.io), the realtime messaging service.

Note: This library was created solely for developers who do not want EventMachine as a dependency of their application.  If this is not a requirement for you, then we recommended you use the combined [REST & Realtime gem](https://rubygems.org/gems/ably).

## Documentation

Visit https://www.ably.io/documentation for a complete API reference and more examples. The examples and API below is not exhaustive.

## Installation

The client library is available as a [gem from RubyGems.org](https://rubygems.org/gems/ably-rest).

Add this line to your application's Gemfile:

    gem 'ably-rest'

And then execute:

    $ bundle

Or install it yourself as:

    $ gem install ably-rest

## Using the REST API

All examples assume a client and/or channel has been created as follows:

```ruby
client = Ably::Rest.new(key: 'xxxxx')
channel = client.channel('test')
```

### Publishing a message to a channel

```ruby
channel.publish('myEvent', 'Hello!') #=> true
```

### Querying the History

```ruby
messages_page = channel.history #=> #<Ably::Models::PaginatedResult ...>
messages_page.items.first #=> #<Ably::Models::Message ...>
messages_page.items.first.data # payload for the message
messages_page.next # retrieves the next page => #<Ably::Models::PaginatedResult ...>
messages_page.has_next? # false, there are more pages
```

### Current presence members on a channel

```ruby
members_page = channel.presence.get # => #<Ably::Models::PaginatedResult ...>
members_page.items.first # first member present in this page => #<Ably::Models::PresenceMessage ...>
members_page.items.first.client_id # client ID of first member present
members_page.next # retrieves the next page => #<Ably::Models::PaginatedResult ...>
members_page.has_next? # false, there are more pages
```

### Querying the presence history

```ruby
presence_page = channel.presence.history #=> #<Ably::Models::PaginatedResult ...>
presence_page.items.first #=> #<Ably::Models::PresenceMessage ...>
presence_page.items.first.client_id # client ID of first member
presence_page.next # retrieves the next page => #<Ably::Models::PaginatedResult ...>
```

### Symmetric end-to-end encrypted payloads on a channel

When a 128 bit or 256 bit key is provided to the library, all payloads are encrypted and decrypted automatically using that key on the channel. The secret key is never transmitted to Ably and thus it is the developer's responsibility to distribute a secret key to both publishers and subscribers.

```ruby
secret_key = Ably::Util::Crypto.generate_random_key
channel = client.channels.get('test', cipher: { key: secret_key })
channel.publish nil, "sensitive data" # data will be encrypted before publish
messages_page = channel.history
messages_page.items.first.data #=> "sensitive data"
```

### Generate a Token

Tokens are issued by Ably and are readily usable by any client to connect to Ably:

```ruby
token_details = client.auth.request_token
# => #<Ably::Models::TokenDetails ...>
token_details.token # => "xVLyHw.CLchevH3hF....MDh9ZC_Q"
client = Ably::Rest.new(token: token_details)
```

### Generate a TokenRequest

Token requests are issued by your servers and signed using your private API key. This is the preferred method of authentication as no secrets are ever shared, and the token request can be issued to trusted clients without communicating with Ably.

```ruby
token_request = client.auth.create_token_request(ttl: 3600, client_id: 'jim')
# => {"id"=>...,
#     "clientId"=>"jim",
#     "ttl"=>3600,
#     "timestamp"=>...,
#     "capability"=>"{\"*\":[\"*\"]}",
#     "nonce"=>...,
#     "mac"=>...}

client = Ably::Rest.new(token: token_request)
```

### Fetching your application's stats

```ruby
stats_page = client.stats #=> #<Ably::Models::PaginatedResult ...>
stats_page.items.first = #<Ably::Models::Stats ...>
stats_page.next # retrieves the next page => #<Ably::Models::PaginatedResult ...>
```

### Fetching the Ably service time

```ruby
client.time #=> 2013-12-12 14:23:34 +0000
```

## Support, feedback and troubleshooting

Please visit http://support.ably.io/ for access to our knowledgebase and to ask for any assistance.

You can also view the [community reported Github issues](https://github.com/ably/ably-ruby-rest/issues).

To see what has changed in recent versions of Bundler, see the [CHANGELOG](CHANGELOG.md).

## Contributing

1. Fork it
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am 'Add some feature'`)
4. Ensure you have added suitable tests and the test suite is passing(`bundle exec rspec`)
4. Push to the branch (`git push origin my-new-feature`)
5. Create a new Pull Request

## License

Copyright (c) 2016 Ably Real-time Ltd, Licensed under the Apache License, Version 2.0.  Refer to [LICENSE](LICENSE) for the license terms.

# Ably REST Client Library 0.7.1 Specification

### Ably::Auth
_(see [lib/submodules/ably-ruby/spec/acceptance/rest/auth_spec.rb](./lib/submodules/ably-ruby/spec/acceptance/rest/auth_spec.rb))_
  * using JSON and MsgPack protocol
    * [has immutable options](./lib/submodules/ably-ruby/spec/acceptance/rest/auth_spec.rb#L54)
    * #request_token
      * [returns the requested token](./lib/submodules/ably-ruby/spec/acceptance/rest/auth_spec.rb#L62)
      * with option :client_id
        * [overrides default and uses camelCase notation for all attributes](./lib/submodules/ably-ruby/spec/acceptance/rest/auth_spec.rb#L93)
      * with option :capability
        * [overrides default and uses camelCase notation for all attributes](./lib/submodules/ably-ruby/spec/acceptance/rest/auth_spec.rb#L93)
      * with option :nonce
        * [overrides default and uses camelCase notation for all attributes](./lib/submodules/ably-ruby/spec/acceptance/rest/auth_spec.rb#L93)
      * with option :timestamp
        * [overrides default and uses camelCase notation for all attributes](./lib/submodules/ably-ruby/spec/acceptance/rest/auth_spec.rb#L93)
      * with option :ttl
        * [overrides default and uses camelCase notation for all attributes](./lib/submodules/ably-ruby/spec/acceptance/rest/auth_spec.rb#L93)
      * with :key_id & :key_secret options
        * [key_id is used in request and signing uses key_secret](./lib/submodules/ably-ruby/spec/acceptance/rest/auth_spec.rb#L122)
      * with :query_time option
        * [queries the server for the time](./lib/submodules/ably-ruby/spec/acceptance/rest/auth_spec.rb#L130)
      * without :query_time option
        * [does not query the server for the time](./lib/submodules/ably-ruby/spec/acceptance/rest/auth_spec.rb#L139)
      * with :auth_url option
        * when response is valid
          * [requests a token from :auth_url using an HTTP GET request](./lib/submodules/ably-ruby/spec/acceptance/rest/auth_spec.rb#L186)
          * with :query_params
            * [requests a token from :auth_url with the :query_params](./lib/submodules/ably-ruby/spec/acceptance/rest/auth_spec.rb#L194)
          * with :headers
            * [requests a token from :auth_url with the HTTP headers set](./lib/submodules/ably-ruby/spec/acceptance/rest/auth_spec.rb#L202)
          * with POST
            * [requests a token from :auth_url using an HTTP POST instead of the default GET](./lib/submodules/ably-ruby/spec/acceptance/rest/auth_spec.rb#L210)
        * when response is invalid
          * 500
            * [raises ServerError](./lib/submodules/ably-ruby/spec/acceptance/rest/auth_spec.rb#L223)
          * XML
            * [raises InvalidResponseBody](./lib/submodules/ably-ruby/spec/acceptance/rest/auth_spec.rb#L234)
      * with token_request_block
        * [calls the block when authenticating to obtain the request token](./lib/submodules/ably-ruby/spec/acceptance/rest/auth_spec.rb#L252)
        * [uses the token request from the block when requesting a new token](./lib/submodules/ably-ruby/spec/acceptance/rest/auth_spec.rb#L257)
    * before #authorise has been called
      * [has no current_token](./lib/submodules/ably-ruby/spec/acceptance/rest/auth_spec.rb#L264)
    * #authorise
      * [updates the persisted auth options thare are then used for subsequent authorise requests](./lib/submodules/ably-ruby/spec/acceptance/rest/auth_spec.rb#L311)
      * when called for the first time since the client has been instantiated
        * [passes all options to #request_token](./lib/submodules/ably-ruby/spec/acceptance/rest/auth_spec.rb#L275)
        * [returns a valid token](./lib/submodules/ably-ruby/spec/acceptance/rest/auth_spec.rb#L280)
        * [issues a new token if option :force => true](./lib/submodules/ably-ruby/spec/acceptance/rest/auth_spec.rb#L284)
      * with previous authorisation
        * [does not request a token if current_token has not expired](./lib/submodules/ably-ruby/spec/acceptance/rest/auth_spec.rb#L295)
        * [requests a new token if token is expired](./lib/submodules/ably-ruby/spec/acceptance/rest/auth_spec.rb#L300)
        * [issues a new token if option :force => true](./lib/submodules/ably-ruby/spec/acceptance/rest/auth_spec.rb#L306)
      * with token_request_block
        * [calls the block](./lib/submodules/ably-ruby/spec/acceptance/rest/auth_spec.rb#L327)
        * [uses the token request returned from the block when requesting a new token](./lib/submodules/ably-ruby/spec/acceptance/rest/auth_spec.rb#L331)
        * for every subsequent #request_token
          * without a provided block
            * [calls the originally provided block](./lib/submodules/ably-ruby/spec/acceptance/rest/auth_spec.rb#L337)
          * with a provided block
            * [does not call the originally provided block and calls the new #request_token block](./lib/submodules/ably-ruby/spec/acceptance/rest/auth_spec.rb#L344)
    * #create_token_request
      * [uses the key ID from the client](./lib/submodules/ably-ruby/spec/acceptance/rest/auth_spec.rb#L360)
      * [uses the default TTL](./lib/submodules/ably-ruby/spec/acceptance/rest/auth_spec.rb#L364)
      * [uses the default capability](./lib/submodules/ably-ruby/spec/acceptance/rest/auth_spec.rb#L368)
      * the nonce
        * [is unique for every request](./lib/submodules/ably-ruby/spec/acceptance/rest/auth_spec.rb#L373)
        * [is at least 16 characters](./lib/submodules/ably-ruby/spec/acceptance/rest/auth_spec.rb#L378)
      * with option :ttl
        * [overrides default](./lib/submodules/ably-ruby/spec/acceptance/rest/auth_spec.rb#L389)
      * with option :capability
        * [overrides default](./lib/submodules/ably-ruby/spec/acceptance/rest/auth_spec.rb#L389)
      * with option :nonce
        * [overrides default](./lib/submodules/ably-ruby/spec/acceptance/rest/auth_spec.rb#L389)
      * with option :timestamp
        * [overrides default](./lib/submodules/ably-ruby/spec/acceptance/rest/auth_spec.rb#L389)
      * with option :client_id
        * [overrides default](./lib/submodules/ably-ruby/spec/acceptance/rest/auth_spec.rb#L389)
      * with additional invalid attributes
        * [are ignored](./lib/submodules/ably-ruby/spec/acceptance/rest/auth_spec.rb#L397)
      * when required fields are missing
        * [should raise an exception if key secret is missing](./lib/submodules/ably-ruby/spec/acceptance/rest/auth_spec.rb#L408)
        * [should raise an exception if key id is missing](./lib/submodules/ably-ruby/spec/acceptance/rest/auth_spec.rb#L412)
      * with :query_time option
        * [queries the server for the timestamp](./lib/submodules/ably-ruby/spec/acceptance/rest/auth_spec.rb#L421)
      * with :timestamp option
        * [uses the provided timestamp in the token request](./lib/submodules/ably-ruby/spec/acceptance/rest/auth_spec.rb#L431)
      * signing
        * [generates a valid HMAC](./lib/submodules/ably-ruby/spec/acceptance/rest/auth_spec.rb#L448)
    * using token authentication
      * with :token_id option
        * [authenticates successfully using the provided :token_id](./lib/submodules/ably-ruby/spec/acceptance/rest/auth_spec.rb#L471)
        * [disallows publishing on unspecified capability channels](./lib/submodules/ably-ruby/spec/acceptance/rest/auth_spec.rb#L475)
        * [fails if timestamp is invalid](./lib/submodules/ably-ruby/spec/acceptance/rest/auth_spec.rb#L483)
        * [cannot be renewed automatically](./lib/submodules/ably-ruby/spec/acceptance/rest/auth_spec.rb#L491)
      * when implicit as a result of using :client id
        * and requests to the Ably server are mocked
          * [will send a token request to the server](./lib/submodules/ably-ruby/spec/acceptance/rest/auth_spec.rb#L521)
        * a token is created
          * [before a request is made](./lib/submodules/ably-ruby/spec/acceptance/rest/auth_spec.rb#L530)
          * [when a message is published](./lib/submodules/ably-ruby/spec/acceptance/rest/auth_spec.rb#L534)
          * [with capability and TTL defaults](./lib/submodules/ably-ruby/spec/acceptance/rest/auth_spec.rb#L538)
    * when using an :api_key and basic auth
      * [#using_token_auth? is false](./lib/submodules/ably-ruby/spec/acceptance/rest/auth_spec.rb#L553)
      * [#using_basic_auth? is true](./lib/submodules/ably-ruby/spec/acceptance/rest/auth_spec.rb#L557)

### Ably::Rest
_(see [lib/submodules/ably-ruby/spec/acceptance/rest/base_spec.rb](./lib/submodules/ably-ruby/spec/acceptance/rest/base_spec.rb))_
  * transport protocol
    * when protocol is not defined it defaults to :msgpack
      * [uses MsgPack](./lib/submodules/ably-ruby/spec/acceptance/rest/base_spec.rb#L27)
    * when option {:protocol=>:json} is used
      * [uses JSON](./lib/submodules/ably-ruby/spec/acceptance/rest/base_spec.rb#L43)
    * when option {:use_binary_protocol=>false} is used
      * [uses JSON](./lib/submodules/ably-ruby/spec/acceptance/rest/base_spec.rb#L43)
    * when option {:protocol=>:msgpack} is used
      * [uses MsgPack](./lib/submodules/ably-ruby/spec/acceptance/rest/base_spec.rb#L60)
    * when option {:use_binary_protocol=>true} is used
      * [uses MsgPack](./lib/submodules/ably-ruby/spec/acceptance/rest/base_spec.rb#L60)
  * using JSON and MsgPack protocol
    * failed requests
      * due to invalid Auth
        * [should raise an InvalidRequest exception with a valid error message and code](./lib/submodules/ably-ruby/spec/acceptance/rest/base_spec.rb#L75)
      * server error with JSON error response body
        * [should raise a ServerError exception](./lib/submodules/ably-ruby/spec/acceptance/rest/base_spec.rb#L94)
      * 500 server error without a valid JSON response body
        * [should raise a ServerError exception](./lib/submodules/ably-ruby/spec/acceptance/rest/base_spec.rb#L105)
    * token authentication failures
      * when auth#token_renewable?
        * [should automatically reissue a token](./lib/submodules/ably-ruby/spec/acceptance/rest/base_spec.rb#L143)
      * when NOT auth#token_renewable?
        * [should raise an InvalidToken exception](./lib/submodules/ably-ruby/spec/acceptance/rest/base_spec.rb#L156)

### Ably::Rest::Channel
_(see [lib/submodules/ably-ruby/spec/acceptance/rest/channel_spec.rb](./lib/submodules/ably-ruby/spec/acceptance/rest/channel_spec.rb))_
  * using JSON and MsgPack protocol
    * #publish
      * [should publish the message adn return true indicating success](./lib/submodules/ably-ruby/spec/acceptance/rest/channel_spec.rb#L17)
    * #history
      * [should return the current message history for the channel](./lib/submodules/ably-ruby/spec/acceptance/rest/channel_spec.rb#L39)
      * [should return paged history using the PaginatedResource model](./lib/submodules/ably-ruby/spec/acceptance/rest/channel_spec.rb#L67)
      * message timestamps
        * [should all be after the messages were published](./lib/submodules/ably-ruby/spec/acceptance/rest/channel_spec.rb#L52)
      * message IDs
        * [should be unique](./lib/submodules/ably-ruby/spec/acceptance/rest/channel_spec.rb#L60)
    * #history option
      * :start
        * with milliseconds since epoch value
          * [uses this value in the history request](./lib/submodules/ably-ruby/spec/acceptance/rest/channel_spec.rb#L116)
        * with a Time object value
          * [converts the value to milliseconds since epoch in the hisotry request](./lib/submodules/ably-ruby/spec/acceptance/rest/channel_spec.rb#L126)
      * :end
        * with milliseconds since epoch value
          * [uses this value in the history request](./lib/submodules/ably-ruby/spec/acceptance/rest/channel_spec.rb#L116)
        * with a Time object value
          * [converts the value to milliseconds since epoch in the hisotry request](./lib/submodules/ably-ruby/spec/acceptance/rest/channel_spec.rb#L126)

### Ably::Rest::Channels
_(see [lib/submodules/ably-ruby/spec/acceptance/rest/channels_spec.rb](./lib/submodules/ably-ruby/spec/acceptance/rest/channels_spec.rb))_
  * using JSON and MsgPack protocol
    * using shortcut method #channel on the client object
      * behaves like a channel
        * [returns a channel object](./lib/submodules/ably-ruby/spec/acceptance/rest/channels_spec.rb#L6)
        * [returns channel object and passes the provided options](./lib/submodules/ably-ruby/spec/acceptance/rest/channels_spec.rb#L11)
    * using #get method on client#channels
      * behaves like a channel
        * [returns a channel object](./lib/submodules/ably-ruby/spec/acceptance/rest/channels_spec.rb#L6)
        * [returns channel object and passes the provided options](./lib/submodules/ably-ruby/spec/acceptance/rest/channels_spec.rb#L11)
    * using undocumented array accessor [] method on client#channels
      * behaves like a channel
        * [returns a channel object](./lib/submodules/ably-ruby/spec/acceptance/rest/channels_spec.rb#L6)
        * [returns channel object and passes the provided options](./lib/submodules/ably-ruby/spec/acceptance/rest/channels_spec.rb#L11)

### Ably::Rest::Client
_(see [lib/submodules/ably-ruby/spec/acceptance/rest/client_spec.rb](./lib/submodules/ably-ruby/spec/acceptance/rest/client_spec.rb))_
  * using JSON and MsgPack protocol
    * #initialize
      * with an auth block
        * [calls the block to get a new token](./lib/submodules/ably-ruby/spec/acceptance/rest/client_spec.rb#L20)
      * with an auth URL
        * [sends an HTTP request to the provided URL to get a new token](./lib/submodules/ably-ruby/spec/acceptance/rest/client_spec.rb#L34)
    * using tokens
      * when expired
        * [creates a new token automatically when the old token expires](./lib/submodules/ably-ruby/spec/acceptance/rest/client_spec.rb#L55)
      * when token has not expired
        * [reuses the existing token for every request](./lib/submodules/ably-ruby/spec/acceptance/rest/client_spec.rb#L69)
    * connection transport
      * for default host
        * [is configured to timeout connection opening in 4 seconds](./lib/submodules/ably-ruby/spec/acceptance/rest/client_spec.rb#L85)
        * [is configured to timeout connection requests in 15 seconds](./lib/submodules/ably-ruby/spec/acceptance/rest/client_spec.rb#L89)
      * for the fallback hosts
        * [is configured to timeout connection opening in 4 seconds](./lib/submodules/ably-ruby/spec/acceptance/rest/client_spec.rb#L95)
        * [is configured to timeout connection requests in 15 seconds](./lib/submodules/ably-ruby/spec/acceptance/rest/client_spec.rb#L99)
    * fallback hosts
      * configured
        * [should make connection attempts to A.ably-realtime.com, B.ably-realtime.com, C.ably-realtime.com, D.ably-realtime.com, E.ably-realtime.com](./lib/submodules/ably-ruby/spec/acceptance/rest/client_spec.rb#L112)
      * when environment is NOT production
        * [does not retry failed requests with fallback hosts when there is a connection error](./lib/submodules/ably-ruby/spec/acceptance/rest/client_spec.rb#L129)
      * when environment is production
        * and connection times out
          * [tries fallback hosts 3 times](./lib/submodules/ably-ruby/spec/acceptance/rest/client_spec.rb#L169)
          * and the total request time exeeds 10 seconds
            * [makes no further attempts to any fallback hosts](./lib/submodules/ably-ruby/spec/acceptance/rest/client_spec.rb#L184)
        * and connection fails
          * [tries fallback hosts 3 times](./lib/submodules/ably-ruby/spec/acceptance/rest/client_spec.rb#L200)
    * with a custom host
      * that does not exist
        * [fails immediately and raises a Faraday Error](./lib/submodules/ably-ruby/spec/acceptance/rest/client_spec.rb#L216)
        * fallback hosts
          * [are never used](./lib/submodules/ably-ruby/spec/acceptance/rest/client_spec.rb#L237)
      * that times out
        * [fails immediately and raises a Faraday Error](./lib/submodules/ably-ruby/spec/acceptance/rest/client_spec.rb#L252)
        * fallback hosts
          * [are never used](./lib/submodules/ably-ruby/spec/acceptance/rest/client_spec.rb#L265)

### Ably::Models::MessageEncoders
_(see [lib/submodules/ably-ruby/spec/acceptance/rest/encoders_spec.rb](./lib/submodules/ably-ruby/spec/acceptance/rest/encoders_spec.rb))_
  * with binary transport protocol
    * without encryption
      * with UTF-8 data
        * [does not apply any encoding](./lib/submodules/ably-ruby/spec/acceptance/rest/encoders_spec.rb#L41)
      * with binary data
        * [does not apply any encoding](./lib/submodules/ably-ruby/spec/acceptance/rest/encoders_spec.rb#L52)
      * with JSON data
        * [stringifies the JSON and sets the json encoding type](./lib/submodules/ably-ruby/spec/acceptance/rest/encoders_spec.rb#L63)
    * with encryption
      * with UTF-8 data
        * [applies utf-8 and cipher encoding](./lib/submodules/ably-ruby/spec/acceptance/rest/encoders_spec.rb#L78)
      * with binary data
        * [applies cipher encoding](./lib/submodules/ably-ruby/spec/acceptance/rest/encoders_spec.rb#L89)
      * with JSON data
        * [applies json, utf-8 and cipher encoding](./lib/submodules/ably-ruby/spec/acceptance/rest/encoders_spec.rb#L100)
  * with text transport protocol
    * without encryption
      * with UTF-8 data
        * [does not apply any encoding](./lib/submodules/ably-ruby/spec/acceptance/rest/encoders_spec.rb#L117)
      * with binary data
        * [applies a base64 encoding](./lib/submodules/ably-ruby/spec/acceptance/rest/encoders_spec.rb#L128)
      * with JSON data
        * [stringifies the JSON and sets the json encoding type](./lib/submodules/ably-ruby/spec/acceptance/rest/encoders_spec.rb#L139)
    * with encryption
      * with UTF-8 data
        * [applies utf-8, cipher and base64 encodings](./lib/submodules/ably-ruby/spec/acceptance/rest/encoders_spec.rb#L154)
      * with binary data
        * [applies cipher and base64 encoding](./lib/submodules/ably-ruby/spec/acceptance/rest/encoders_spec.rb#L165)
      * with JSON data
        * [applies json, utf-8, cipher and base64 encoding](./lib/submodules/ably-ruby/spec/acceptance/rest/encoders_spec.rb#L176)

### Ably::Rest::Channel messages
_(see [lib/submodules/ably-ruby/spec/acceptance/rest/message_spec.rb](./lib/submodules/ably-ruby/spec/acceptance/rest/message_spec.rb))_
  * using JSON and MsgPack protocol
    * publishing with an ASCII_8BIT message name
      * [is converted into UTF_8](./lib/submodules/ably-ruby/spec/acceptance/rest/message_spec.rb#L18)
    * encryption and encoding
      * with #publish and #history
        * with AES-128-CBC using crypto-data-128.json fixtures
          * item 0 with encrypted encoding utf-8/cipher+aes-128-cbc/base64
            * behaves like an Ably encrypter and decrypter
              * [encrypts message automatically when published](./lib/submodules/ably-ruby/spec/acceptance/rest/message_spec.rb#L65)
              * [sends and retrieves messages that are encrypted & decrypted by the Ably library](./lib/submodules/ably-ruby/spec/acceptance/rest/message_spec.rb#L80)
          * item 1 with encrypted encoding cipher+aes-128-cbc/base64
            * behaves like an Ably encrypter and decrypter
              * [encrypts message automatically when published](./lib/submodules/ably-ruby/spec/acceptance/rest/message_spec.rb#L65)
              * [sends and retrieves messages that are encrypted & decrypted by the Ably library](./lib/submodules/ably-ruby/spec/acceptance/rest/message_spec.rb#L80)
          * item 2 with encrypted encoding json/utf-8/cipher+aes-128-cbc/base64
            * behaves like an Ably encrypter and decrypter
              * [encrypts message automatically when published](./lib/submodules/ably-ruby/spec/acceptance/rest/message_spec.rb#L65)
              * [sends and retrieves messages that are encrypted & decrypted by the Ably library](./lib/submodules/ably-ruby/spec/acceptance/rest/message_spec.rb#L80)
          * item 3 with encrypted encoding json/utf-8/cipher+aes-128-cbc/base64
            * behaves like an Ably encrypter and decrypter
              * [encrypts message automatically when published](./lib/submodules/ably-ruby/spec/acceptance/rest/message_spec.rb#L65)
              * [sends and retrieves messages that are encrypted & decrypted by the Ably library](./lib/submodules/ably-ruby/spec/acceptance/rest/message_spec.rb#L80)
        * with AES-256-CBC using crypto-data-256.json fixtures
          * item 0 with encrypted encoding utf-8/cipher+aes-256-cbc/base64
            * behaves like an Ably encrypter and decrypter
              * [encrypts message automatically when published](./lib/submodules/ably-ruby/spec/acceptance/rest/message_spec.rb#L65)
              * [sends and retrieves messages that are encrypted & decrypted by the Ably library](./lib/submodules/ably-ruby/spec/acceptance/rest/message_spec.rb#L80)
          * item 1 with encrypted encoding cipher+aes-256-cbc/base64
            * behaves like an Ably encrypter and decrypter
              * [encrypts message automatically when published](./lib/submodules/ably-ruby/spec/acceptance/rest/message_spec.rb#L65)
              * [sends and retrieves messages that are encrypted & decrypted by the Ably library](./lib/submodules/ably-ruby/spec/acceptance/rest/message_spec.rb#L80)
          * item 2 with encrypted encoding json/utf-8/cipher+aes-256-cbc/base64
            * behaves like an Ably encrypter and decrypter
              * [encrypts message automatically when published](./lib/submodules/ably-ruby/spec/acceptance/rest/message_spec.rb#L65)
              * [sends and retrieves messages that are encrypted & decrypted by the Ably library](./lib/submodules/ably-ruby/spec/acceptance/rest/message_spec.rb#L80)
          * item 3 with encrypted encoding json/utf-8/cipher+aes-256-cbc/base64
            * behaves like an Ably encrypter and decrypter
              * [encrypts message automatically when published](./lib/submodules/ably-ruby/spec/acceptance/rest/message_spec.rb#L65)
              * [sends and retrieves messages that are encrypted & decrypted by the Ably library](./lib/submodules/ably-ruby/spec/acceptance/rest/message_spec.rb#L80)
        * when publishing lots of messages
          * [encrypts on #publish and decrypts on #history](./lib/submodules/ably-ruby/spec/acceptance/rest/message_spec.rb#L113)
        * when retrieving #history with a different protocol
          * [delivers a String ASCII-8BIT payload to the receiver](./lib/submodules/ably-ruby/spec/acceptance/rest/message_spec.rb#L140)
          * [delivers a String UTF-8 payload to the receiver](./lib/submodules/ably-ruby/spec/acceptance/rest/message_spec.rb#L140)
          * [delivers a Hash payload to the receiver](./lib/submodules/ably-ruby/spec/acceptance/rest/message_spec.rb#L140)
        * when publishing on an unencrypted channel and retrieving with #history on an encrypted channel
          * [does not attempt to decrypt the message](./lib/submodules/ably-ruby/spec/acceptance/rest/message_spec.rb#L156)
        * when publishing on an encrypted channel and retrieving with #history on an unencrypted channel
          * [retrieves the message that remains encrypted with an encrypted encoding attribute](./lib/submodules/ably-ruby/spec/acceptance/rest/message_spec.rb#L177)
          * [logs a Cipher exception](./lib/submodules/ably-ruby/spec/acceptance/rest/message_spec.rb#L183)
        * publishing on an encrypted channel and retrieving #history with a different algorithm on another client
          * [retrieves the message that remains encrypted with an encrypted encoding attribute](./lib/submodules/ably-ruby/spec/acceptance/rest/message_spec.rb#L204)
          * [logs a Cipher exception](./lib/submodules/ably-ruby/spec/acceptance/rest/message_spec.rb#L210)
        * publishing on an encrypted channel and subscribing with a different key on another client
          * [retrieves the message that remains encrypted with an encrypted encoding attribute](./lib/submodules/ably-ruby/spec/acceptance/rest/message_spec.rb#L231)
          * [logs a Cipher exception](./lib/submodules/ably-ruby/spec/acceptance/rest/message_spec.rb#L237)

### Ably::Rest::Presence
_(see [lib/submodules/ably-ruby/spec/acceptance/rest/presence_spec.rb](./lib/submodules/ably-ruby/spec/acceptance/rest/presence_spec.rb))_
  * using JSON and MsgPack protocol
    * tested against presence fixture data set up in test app
      * #get
        * [returns current members on the channel with their action set to :present](./lib/submodules/ably-ruby/spec/acceptance/rest/presence_spec.rb#L31)
        * with :limit option
          * [returns a paged response limiting number of members per page](./lib/submodules/ably-ruby/spec/acceptance/rest/presence_spec.rb#L45)
      * #history
        * [returns recent presence activity](./lib/submodules/ably-ruby/spec/acceptance/rest/presence_spec.rb#L58)
        * with options
          * direction: :forwards
            * [returns recent presence activity forwards with most recent history last](./lib/submodules/ably-ruby/spec/acceptance/rest/presence_spec.rb#L74)
          * direction: :backwards
            * [returns recent presence activity backwards with most recent history first](./lib/submodules/ably-ruby/spec/acceptance/rest/presence_spec.rb#L89)
    * #history
      * with time range options
        * :start
          * with milliseconds since epoch value
            * [uses this value in the history request](./lib/submodules/ably-ruby/spec/acceptance/rest/presence_spec.rb#L134)
          * with Time object value
            * [converts the value to milliseconds since epoch in the hisotry request](./lib/submodules/ably-ruby/spec/acceptance/rest/presence_spec.rb#L144)
        * :end
          * with milliseconds since epoch value
            * [uses this value in the history request](./lib/submodules/ably-ruby/spec/acceptance/rest/presence_spec.rb#L134)
          * with Time object value
            * [converts the value to milliseconds since epoch in the hisotry request](./lib/submodules/ably-ruby/spec/acceptance/rest/presence_spec.rb#L144)
    * decoding
      * valid decodeable content
        * #get
          * [automaticaly decodes presence messages](./lib/submodules/ably-ruby/spec/acceptance/rest/presence_spec.rb#L202)
        * #history
          * [automaticaly decodes presence messages](./lib/submodules/ably-ruby/spec/acceptance/rest/presence_spec.rb#L219)
      * invalid data
        * #get
          * [returns the messages still encoded](./lib/submodules/ably-ruby/spec/acceptance/rest/presence_spec.rb#L250)
          * [logs a cipher error](./lib/submodules/ably-ruby/spec/acceptance/rest/presence_spec.rb#L254)
        * #history
          * [returns the messages still encoded](./lib/submodules/ably-ruby/spec/acceptance/rest/presence_spec.rb#L274)
          * [logs a cipher error](./lib/submodules/ably-ruby/spec/acceptance/rest/presence_spec.rb#L278)

### Ably::Rest::Client#stats
_(see [lib/submodules/ably-ruby/spec/acceptance/rest/stats_spec.rb](./lib/submodules/ably-ruby/spec/acceptance/rest/stats_spec.rb))_
  * using JSON and MsgPack protocol
    * fetching application stats
      * by minute
        * [should return all the stats for the application](./lib/submodules/ably-ruby/spec/acceptance/rest/stats_spec.rb#L49)
      * by hour
        * [should return all the stats for the application](./lib/submodules/ably-ruby/spec/acceptance/rest/stats_spec.rb#L49)
      * by day
        * [should return all the stats for the application](./lib/submodules/ably-ruby/spec/acceptance/rest/stats_spec.rb#L49)
      * by month
        * [should return all the stats for the application](./lib/submodules/ably-ruby/spec/acceptance/rest/stats_spec.rb#L49)

### Ably::Rest::Client#time
_(see [lib/submodules/ably-ruby/spec/acceptance/rest/time_spec.rb](./lib/submodules/ably-ruby/spec/acceptance/rest/time_spec.rb))_
  * using JSON and MsgPack protocol
    * fetching the service time
      * [should return the service time as a Time object](./lib/submodules/ably-ruby/spec/acceptance/rest/time_spec.rb#L10)

### Ably::Auth
_(see [lib/submodules/ably-ruby/spec/unit/auth_spec.rb](./lib/submodules/ably-ruby/spec/unit/auth_spec.rb))_
  * client_id option
    * with nil value
      * [is permitted](./lib/submodules/ably-ruby/spec/unit/auth_spec.rb#L19)
    * as UTF_8 string
      * [is permitted](./lib/submodules/ably-ruby/spec/unit/auth_spec.rb#L27)
      * [remains as UTF-8](./lib/submodules/ably-ruby/spec/unit/auth_spec.rb#L31)
    * as SHIFT_JIS string
      * [gets converted to UTF-8](./lib/submodules/ably-ruby/spec/unit/auth_spec.rb#L39)
      * [is compatible with original encoding](./lib/submodules/ably-ruby/spec/unit/auth_spec.rb#L43)
    * as ASCII_8BIT string
      * [gets converted to UTF-8](./lib/submodules/ably-ruby/spec/unit/auth_spec.rb#L51)
      * [is compatible with original encoding](./lib/submodules/ably-ruby/spec/unit/auth_spec.rb#L55)
    * as Integer
      * [raises an argument error](./lib/submodules/ably-ruby/spec/unit/auth_spec.rb#L63)

### Ably::Logger
_(see [lib/submodules/ably-ruby/spec/unit/logger_spec.rb](./lib/submodules/ably-ruby/spec/unit/logger_spec.rb))_
  * [uses the language provided Logger by default](./lib/submodules/ably-ruby/spec/unit/logger_spec.rb#L15)
  * with a custom Logger
    * with an invalid interface
      * [raises an exception](./lib/submodules/ably-ruby/spec/unit/logger_spec.rb#L116)
    * with a valid interface
      * [is used](./lib/submodules/ably-ruby/spec/unit/logger_spec.rb#L135)

### Ably::Models::ErrorInfo
_(see [lib/submodules/ably-ruby/spec/unit/models/error_info_spec.rb](./lib/submodules/ably-ruby/spec/unit/models/error_info_spec.rb))_
  * behaves like a model
    * attributes
      * #code
        * [retrieves attribute :code](./lib/submodules/ably-ruby/spec/shared/model_behaviour.rb#L15)
      * #status_code
        * [retrieves attribute :status_code](./lib/submodules/ably-ruby/spec/shared/model_behaviour.rb#L15)
      * #message
        * [retrieves attribute :message](./lib/submodules/ably-ruby/spec/shared/model_behaviour.rb#L15)
    * #==
      * [is true when attributes are the same](./lib/submodules/ably-ruby/spec/shared/model_behaviour.rb#L41)
      * [is false when attributes are not the same](./lib/submodules/ably-ruby/spec/shared/model_behaviour.rb#L46)
      * [is false when class type differs](./lib/submodules/ably-ruby/spec/shared/model_behaviour.rb#L50)
    * is immutable
      * [prevents changes](./lib/submodules/ably-ruby/spec/shared/model_behaviour.rb#L76)
      * [dups options](./lib/submodules/ably-ruby/spec/shared/model_behaviour.rb#L80)
  * #status
    * [is an alias for #status_code](./lib/submodules/ably-ruby/spec/unit/models/error_info_spec.rb#L13)

### Ably::Models::Message
_(see [lib/submodules/ably-ruby/spec/unit/models/message_spec.rb](./lib/submodules/ably-ruby/spec/unit/models/message_spec.rb))_
  * behaves like a model
    * attributes
      * #name
        * [retrieves attribute :name](./lib/submodules/ably-ruby/spec/shared/model_behaviour.rb#L15)
      * #client_id
        * [retrieves attribute :client_id](./lib/submodules/ably-ruby/spec/shared/model_behaviour.rb#L15)
      * #data
        * [retrieves attribute :data](./lib/submodules/ably-ruby/spec/shared/model_behaviour.rb#L15)
      * #encoding
        * [retrieves attribute :encoding](./lib/submodules/ably-ruby/spec/shared/model_behaviour.rb#L15)
    * #==
      * [is true when attributes are the same](./lib/submodules/ably-ruby/spec/shared/model_behaviour.rb#L41)
      * [is false when attributes are not the same](./lib/submodules/ably-ruby/spec/shared/model_behaviour.rb#L46)
      * [is false when class type differs](./lib/submodules/ably-ruby/spec/shared/model_behaviour.rb#L50)
    * is immutable
      * [prevents changes](./lib/submodules/ably-ruby/spec/shared/model_behaviour.rb#L76)
      * [dups options](./lib/submodules/ably-ruby/spec/shared/model_behaviour.rb#L80)
  * #timestamp
    * [retrieves attribute :timestamp as Time object from ProtocolMessage](./lib/submodules/ably-ruby/spec/unit/models/message_spec.rb#L21)
  * #connection_id attribute
    * when this model has a connectionId attribute
      * but no protocol message
        * [uses the model value](./lib/submodules/ably-ruby/spec/unit/models/message_spec.rb#L36)
      * with a protocol message with a different connectionId
        * [uses the model value](./lib/submodules/ably-ruby/spec/unit/models/message_spec.rb#L44)
    * when this model has no connectionId attribute
      * and no protocol message
        * [uses the model value](./lib/submodules/ably-ruby/spec/unit/models/message_spec.rb#L54)
      * with a protocol message with a connectionId
        * [uses the model value](./lib/submodules/ably-ruby/spec/unit/models/message_spec.rb#L62)
  * initialized with
    * :name
      * as UTF_8 string
        * [is permitted](./lib/submodules/ably-ruby/spec/unit/models/message_spec.rb#L89)
        * [remains as UTF-8](./lib/submodules/ably-ruby/spec/unit/models/message_spec.rb#L93)
      * as SHIFT_JIS string
        * [gets converted to UTF-8](./lib/submodules/ably-ruby/spec/unit/models/message_spec.rb#L101)
        * [is compatible with original encoding](./lib/submodules/ably-ruby/spec/unit/models/message_spec.rb#L105)
      * as ASCII_8BIT string
        * [gets converted to UTF-8](./lib/submodules/ably-ruby/spec/unit/models/message_spec.rb#L113)
        * [is compatible with original encoding](./lib/submodules/ably-ruby/spec/unit/models/message_spec.rb#L117)
      * as Integer
        * [raises an argument error](./lib/submodules/ably-ruby/spec/unit/models/message_spec.rb#L125)
      * as Nil
        * [is permitted](./lib/submodules/ably-ruby/spec/unit/models/message_spec.rb#L133)
    * :client_id
      * as UTF_8 string
        * [is permitted](./lib/submodules/ably-ruby/spec/unit/models/message_spec.rb#L89)
        * [remains as UTF-8](./lib/submodules/ably-ruby/spec/unit/models/message_spec.rb#L93)
      * as SHIFT_JIS string
        * [gets converted to UTF-8](./lib/submodules/ably-ruby/spec/unit/models/message_spec.rb#L101)
        * [is compatible with original encoding](./lib/submodules/ably-ruby/spec/unit/models/message_spec.rb#L105)
      * as ASCII_8BIT string
        * [gets converted to UTF-8](./lib/submodules/ably-ruby/spec/unit/models/message_spec.rb#L113)
        * [is compatible with original encoding](./lib/submodules/ably-ruby/spec/unit/models/message_spec.rb#L117)
      * as Integer
        * [raises an argument error](./lib/submodules/ably-ruby/spec/unit/models/message_spec.rb#L125)
      * as Nil
        * [is permitted](./lib/submodules/ably-ruby/spec/unit/models/message_spec.rb#L133)
    * :encoding
      * as UTF_8 string
        * [is permitted](./lib/submodules/ably-ruby/spec/unit/models/message_spec.rb#L89)
        * [remains as UTF-8](./lib/submodules/ably-ruby/spec/unit/models/message_spec.rb#L93)
      * as SHIFT_JIS string
        * [gets converted to UTF-8](./lib/submodules/ably-ruby/spec/unit/models/message_spec.rb#L101)
        * [is compatible with original encoding](./lib/submodules/ably-ruby/spec/unit/models/message_spec.rb#L105)
      * as ASCII_8BIT string
        * [gets converted to UTF-8](./lib/submodules/ably-ruby/spec/unit/models/message_spec.rb#L113)
        * [is compatible with original encoding](./lib/submodules/ably-ruby/spec/unit/models/message_spec.rb#L117)
      * as Integer
        * [raises an argument error](./lib/submodules/ably-ruby/spec/unit/models/message_spec.rb#L125)
      * as Nil
        * [is permitted](./lib/submodules/ably-ruby/spec/unit/models/message_spec.rb#L133)

### Ably::Models::PaginatedResource
_(see [lib/submodules/ably-ruby/spec/unit/models/paginated_resource_spec.rb](./lib/submodules/ably-ruby/spec/unit/models/paginated_resource_spec.rb))_
  * [returns correct length from body](./lib/submodules/ably-ruby/spec/unit/models/paginated_resource_spec.rb#L30)
  * [supports alias methods for length](./lib/submodules/ably-ruby/spec/unit/models/paginated_resource_spec.rb#L34)
  * [is Enumerable](./lib/submodules/ably-ruby/spec/unit/models/paginated_resource_spec.rb#L39)
  * [is iterable](./lib/submodules/ably-ruby/spec/unit/models/paginated_resource_spec.rb#L43)
  * [provides [] accessor method](./lib/submodules/ably-ruby/spec/unit/models/paginated_resource_spec.rb#L61)
  * [#first gets the first item in page](./lib/submodules/ably-ruby/spec/unit/models/paginated_resource_spec.rb#L67)
  * [#last gets the last item in page](./lib/submodules/ably-ruby/spec/unit/models/paginated_resource_spec.rb#L71)
  * #each
    * [returns an enumerator](./lib/submodules/ably-ruby/spec/unit/models/paginated_resource_spec.rb#L48)
    * [yields each item](./lib/submodules/ably-ruby/spec/unit/models/paginated_resource_spec.rb#L52)
  * with non paged http response
    * [is the first page](./lib/submodules/ably-ruby/spec/unit/models/paginated_resource_spec.rb#L175)
    * [is the last page](./lib/submodules/ably-ruby/spec/unit/models/paginated_resource_spec.rb#L179)
    * [does not support pagination](./lib/submodules/ably-ruby/spec/unit/models/paginated_resource_spec.rb#L183)
    * [raises an exception when accessing next page](./lib/submodules/ably-ruby/spec/unit/models/paginated_resource_spec.rb#L187)
    * [raises an exception when accessing first page](./lib/submodules/ably-ruby/spec/unit/models/paginated_resource_spec.rb#L191)
  * with paged http response
    * [is the first page](./lib/submodules/ably-ruby/spec/unit/models/paginated_resource_spec.rb#L209)
    * [is not the last page](./lib/submodules/ably-ruby/spec/unit/models/paginated_resource_spec.rb#L213)
    * [supports pagination](./lib/submodules/ably-ruby/spec/unit/models/paginated_resource_spec.rb#L217)
    * accessing next page
      * [returns another PaginatedResource](./lib/submodules/ably-ruby/spec/unit/models/paginated_resource_spec.rb#L245)
      * [retrieves the next page of results](./lib/submodules/ably-ruby/spec/unit/models/paginated_resource_spec.rb#L249)
      * [is not the first page](./lib/submodules/ably-ruby/spec/unit/models/paginated_resource_spec.rb#L254)
      * [is the last page](./lib/submodules/ably-ruby/spec/unit/models/paginated_resource_spec.rb#L258)
      * [raises an exception if trying to access the last page when it is the last page](./lib/submodules/ably-ruby/spec/unit/models/paginated_resource_spec.rb#L262)
      * and then first page
        * [returns a PaginatedResource](./lib/submodules/ably-ruby/spec/unit/models/paginated_resource_spec.rb#L273)
        * [retrieves the first page of results](./lib/submodules/ably-ruby/spec/unit/models/paginated_resource_spec.rb#L277)
        * [is the first page](./lib/submodules/ably-ruby/spec/unit/models/paginated_resource_spec.rb#L281)

### Ably::Models::PresenceMessage
_(see [lib/submodules/ably-ruby/spec/unit/models/presence_message_spec.rb](./lib/submodules/ably-ruby/spec/unit/models/presence_message_spec.rb))_
  * behaves like a model
    * attributes
      * #client_id
        * [retrieves attribute :client_id](./lib/submodules/ably-ruby/spec/shared/model_behaviour.rb#L15)
      * #data
        * [retrieves attribute :data](./lib/submodules/ably-ruby/spec/shared/model_behaviour.rb#L15)
      * #encoding
        * [retrieves attribute :encoding](./lib/submodules/ably-ruby/spec/shared/model_behaviour.rb#L15)
    * #==
      * [is true when attributes are the same](./lib/submodules/ably-ruby/spec/shared/model_behaviour.rb#L41)
      * [is false when attributes are not the same](./lib/submodules/ably-ruby/spec/shared/model_behaviour.rb#L46)
      * [is false when class type differs](./lib/submodules/ably-ruby/spec/shared/model_behaviour.rb#L50)
    * is immutable
      * [prevents changes](./lib/submodules/ably-ruby/spec/shared/model_behaviour.rb#L76)
      * [dups options](./lib/submodules/ably-ruby/spec/shared/model_behaviour.rb#L80)
  * #connection_id attribute
    * when this model has a connectionId attribute
      * but no protocol message
        * [uses the model value](./lib/submodules/ably-ruby/spec/unit/models/presence_message_spec.rb#L25)
      * with a protocol message with a different connectionId
        * [uses the model value](./lib/submodules/ably-ruby/spec/unit/models/presence_message_spec.rb#L33)
    * when this model has no connectionId attribute
      * and no protocol message
        * [uses the model value](./lib/submodules/ably-ruby/spec/unit/models/presence_message_spec.rb#L43)
      * with a protocol message with a connectionId
        * [uses the model value](./lib/submodules/ably-ruby/spec/unit/models/presence_message_spec.rb#L51)
  * #member_key attribute
    * [is string in format connection_id:client_id](./lib/submodules/ably-ruby/spec/unit/models/presence_message_spec.rb#L61)
    * with the same client id across multiple connections
      * [is unique](./lib/submodules/ably-ruby/spec/unit/models/presence_message_spec.rb#L69)
    * with a single connection and different client_ids
      * [is unique](./lib/submodules/ably-ruby/spec/unit/models/presence_message_spec.rb#L78)
  * #timestamp
    * [retrieves attribute :timestamp as a Time object from ProtocolMessage](./lib/submodules/ably-ruby/spec/unit/models/presence_message_spec.rb#L86)
  * initialized with
    * :client_id
      * as UTF_8 string
        * [is permitted](./lib/submodules/ably-ruby/spec/unit/models/presence_message_spec.rb#L138)
        * [remains as UTF-8](./lib/submodules/ably-ruby/spec/unit/models/presence_message_spec.rb#L142)
      * as SHIFT_JIS string
        * [gets converted to UTF-8](./lib/submodules/ably-ruby/spec/unit/models/presence_message_spec.rb#L150)
        * [is compatible with original encoding](./lib/submodules/ably-ruby/spec/unit/models/presence_message_spec.rb#L154)
      * as ASCII_8BIT string
        * [gets converted to UTF-8](./lib/submodules/ably-ruby/spec/unit/models/presence_message_spec.rb#L162)
        * [is compatible with original encoding](./lib/submodules/ably-ruby/spec/unit/models/presence_message_spec.rb#L166)
      * as Integer
        * [raises an argument error](./lib/submodules/ably-ruby/spec/unit/models/presence_message_spec.rb#L174)
      * as Nil
        * [is permitted](./lib/submodules/ably-ruby/spec/unit/models/presence_message_spec.rb#L182)
    * :connection_id
      * as UTF_8 string
        * [is permitted](./lib/submodules/ably-ruby/spec/unit/models/presence_message_spec.rb#L138)
        * [remains as UTF-8](./lib/submodules/ably-ruby/spec/unit/models/presence_message_spec.rb#L142)
      * as SHIFT_JIS string
        * [gets converted to UTF-8](./lib/submodules/ably-ruby/spec/unit/models/presence_message_spec.rb#L150)
        * [is compatible with original encoding](./lib/submodules/ably-ruby/spec/unit/models/presence_message_spec.rb#L154)
      * as ASCII_8BIT string
        * [gets converted to UTF-8](./lib/submodules/ably-ruby/spec/unit/models/presence_message_spec.rb#L162)
        * [is compatible with original encoding](./lib/submodules/ably-ruby/spec/unit/models/presence_message_spec.rb#L166)
      * as Integer
        * [raises an argument error](./lib/submodules/ably-ruby/spec/unit/models/presence_message_spec.rb#L174)
      * as Nil
        * [is permitted](./lib/submodules/ably-ruby/spec/unit/models/presence_message_spec.rb#L182)
    * :encoding
      * as UTF_8 string
        * [is permitted](./lib/submodules/ably-ruby/spec/unit/models/presence_message_spec.rb#L138)
        * [remains as UTF-8](./lib/submodules/ably-ruby/spec/unit/models/presence_message_spec.rb#L142)
      * as SHIFT_JIS string
        * [gets converted to UTF-8](./lib/submodules/ably-ruby/spec/unit/models/presence_message_spec.rb#L150)
        * [is compatible with original encoding](./lib/submodules/ably-ruby/spec/unit/models/presence_message_spec.rb#L154)
      * as ASCII_8BIT string
        * [gets converted to UTF-8](./lib/submodules/ably-ruby/spec/unit/models/presence_message_spec.rb#L162)
        * [is compatible with original encoding](./lib/submodules/ably-ruby/spec/unit/models/presence_message_spec.rb#L166)
      * as Integer
        * [raises an argument error](./lib/submodules/ably-ruby/spec/unit/models/presence_message_spec.rb#L174)
      * as Nil
        * [is permitted](./lib/submodules/ably-ruby/spec/unit/models/presence_message_spec.rb#L182)

### Ably::Models::ProtocolMessage
_(see [lib/submodules/ably-ruby/spec/unit/models/protocol_message_spec.rb](./lib/submodules/ably-ruby/spec/unit/models/protocol_message_spec.rb))_
  * behaves like a model
    * attributes
      * #id
        * [retrieves attribute :id](./lib/submodules/ably-ruby/spec/shared/model_behaviour.rb#L15)
      * #channel
        * [retrieves attribute :channel](./lib/submodules/ably-ruby/spec/shared/model_behaviour.rb#L15)
      * #channel_serial
        * [retrieves attribute :channel_serial](./lib/submodules/ably-ruby/spec/shared/model_behaviour.rb#L15)
      * #connection_id
        * [retrieves attribute :connection_id](./lib/submodules/ably-ruby/spec/shared/model_behaviour.rb#L15)
    * #==
      * [is true when attributes are the same](./lib/submodules/ably-ruby/spec/shared/model_behaviour.rb#L41)
      * [is false when attributes are not the same](./lib/submodules/ably-ruby/spec/shared/model_behaviour.rb#L46)
      * [is false when class type differs](./lib/submodules/ably-ruby/spec/shared/model_behaviour.rb#L50)
    * is immutable
      * [prevents changes](./lib/submodules/ably-ruby/spec/shared/model_behaviour.rb#L76)
      * [dups options](./lib/submodules/ably-ruby/spec/shared/model_behaviour.rb#L80)
  * attributes
    * #timestamp
      * [retrieves attribute :timestamp as Time object](./lib/submodules/ably-ruby/spec/unit/models/protocol_message_spec.rb#L74)
    * #count
      * when missing
        * [is 1](./lib/submodules/ably-ruby/spec/unit/models/protocol_message_spec.rb#L83)
      * when non numeric
        * [is 1](./lib/submodules/ably-ruby/spec/unit/models/protocol_message_spec.rb#L90)
      * when greater than 1
        * [is the value of count](./lib/submodules/ably-ruby/spec/unit/models/protocol_message_spec.rb#L97)
    * #message_serial
      * [converts :msg_serial to an Integer](./lib/submodules/ably-ruby/spec/unit/models/protocol_message_spec.rb#L105)
    * #has_message_serial?
      * without msg_serial
        * [returns false](./lib/submodules/ably-ruby/spec/unit/models/protocol_message_spec.rb#L115)
      * with msg_serial
        * [returns true](./lib/submodules/ably-ruby/spec/unit/models/protocol_message_spec.rb#L123)
    * #connection_serial
      * [converts :connection_serial to an Integer](./lib/submodules/ably-ruby/spec/unit/models/protocol_message_spec.rb#L131)
    * #flags
      * when nil
        * [is zero](./lib/submodules/ably-ruby/spec/unit/models/protocol_message_spec.rb#L141)
      * when numeric
        * [is an Integer](./lib/submodules/ably-ruby/spec/unit/models/protocol_message_spec.rb#L149)
      * when has_presence
        * [#has_presence_flag? is true](./lib/submodules/ably-ruby/spec/unit/models/protocol_message_spec.rb#L157)
      * when has another future flag
        * [#has_presence_flag? is false](./lib/submodules/ably-ruby/spec/unit/models/protocol_message_spec.rb#L165)
    * #has_connection_serial?
      * without connection_serial
        * [returns false](./lib/submodules/ably-ruby/spec/unit/models/protocol_message_spec.rb#L175)
      * with connection_serial
        * [returns true](./lib/submodules/ably-ruby/spec/unit/models/protocol_message_spec.rb#L183)
    * #serial
      * with underlying msg_serial
        * [converts :msg_serial to an Integer](./lib/submodules/ably-ruby/spec/unit/models/protocol_message_spec.rb#L192)
      * with underlying connection_serial
        * [converts :connection_serial to an Integer](./lib/submodules/ably-ruby/spec/unit/models/protocol_message_spec.rb#L200)
      * with underlying connection_serial and msg_serial
        * [prefers connection_serial and converts :connection_serial to an Integer](./lib/submodules/ably-ruby/spec/unit/models/protocol_message_spec.rb#L208)
    * #has_serial?
      * without msg_serial or connection_serial
        * [returns false](./lib/submodules/ably-ruby/spec/unit/models/protocol_message_spec.rb#L219)
      * with msg_serial
        * [returns true](./lib/submodules/ably-ruby/spec/unit/models/protocol_message_spec.rb#L227)
      * with connection_serial
        * [returns true](./lib/submodules/ably-ruby/spec/unit/models/protocol_message_spec.rb#L235)
    * #error
      * with no error attribute
        * [returns nil](./lib/submodules/ably-ruby/spec/unit/models/protocol_message_spec.rb#L245)
      * with nil error
        * [returns nil](./lib/submodules/ably-ruby/spec/unit/models/protocol_message_spec.rb#L253)
      * with error
        * [returns a valid ErrorInfo object](./lib/submodules/ably-ruby/spec/unit/models/protocol_message_spec.rb#L261)

### Ably::Models::Token
_(see [lib/submodules/ably-ruby/spec/unit/models/token_spec.rb](./lib/submodules/ably-ruby/spec/unit/models/token_spec.rb))_
  * behaves like a model
    * attributes
      * #id
        * [retrieves attribute :id](./lib/submodules/ably-ruby/spec/shared/model_behaviour.rb#L15)
      * #capability
        * [retrieves attribute :capability](./lib/submodules/ably-ruby/spec/shared/model_behaviour.rb#L15)
      * #client_id
        * [retrieves attribute :client_id](./lib/submodules/ably-ruby/spec/shared/model_behaviour.rb#L15)
      * #nonce
        * [retrieves attribute :nonce](./lib/submodules/ably-ruby/spec/shared/model_behaviour.rb#L15)
    * #==
      * [is true when attributes are the same](./lib/submodules/ably-ruby/spec/shared/model_behaviour.rb#L41)
      * [is false when attributes are not the same](./lib/submodules/ably-ruby/spec/shared/model_behaviour.rb#L46)
      * [is false when class type differs](./lib/submodules/ably-ruby/spec/shared/model_behaviour.rb#L50)
    * is immutable
      * [prevents changes](./lib/submodules/ably-ruby/spec/shared/model_behaviour.rb#L76)
      * [dups options](./lib/submodules/ably-ruby/spec/shared/model_behaviour.rb#L80)
  * defaults
    * [should default TTL to 1 hour](./lib/submodules/ably-ruby/spec/unit/models/token_spec.rb#L14)
    * [should default capability to all](./lib/submodules/ably-ruby/spec/unit/models/token_spec.rb#L18)
    * [should only have defaults for :ttl and :capability](./lib/submodules/ably-ruby/spec/unit/models/token_spec.rb#L22)
  * attributes
    * #key_id
      * [retrieves attribute :key](./lib/submodules/ably-ruby/spec/unit/models/token_spec.rb#L32)
    * #issued_at
      * [retrieves attribute :issued_at as Time](./lib/submodules/ably-ruby/spec/unit/models/token_spec.rb#L42)
    * #expires_at
      * [retrieves attribute :expires as Time](./lib/submodules/ably-ruby/spec/unit/models/token_spec.rb#L42)
    * #expired?
      * once grace period buffer has passed
        * [is true](./lib/submodules/ably-ruby/spec/unit/models/token_spec.rb#L55)
      * within grace period buffer
        * [is false](./lib/submodules/ably-ruby/spec/unit/models/token_spec.rb#L63)
  * ==
    * [is true when attributes are the same](./lib/submodules/ably-ruby/spec/unit/models/token_spec.rb#L73)
    * [is false when attributes are not the same](./lib/submodules/ably-ruby/spec/unit/models/token_spec.rb#L78)
    * [is false when class type differs](./lib/submodules/ably-ruby/spec/unit/models/token_spec.rb#L82)

### Ably::Rest::Channels
_(see [lib/submodules/ably-ruby/spec/unit/rest/channel_spec.rb](./lib/submodules/ably-ruby/spec/unit/rest/channel_spec.rb))_
  * #initializer
    * as UTF_8 string
      * [is permitted](./lib/submodules/ably-ruby/spec/unit/rest/channel_spec.rb#L16)
      * [remains as UTF-8](./lib/submodules/ably-ruby/spec/unit/rest/channel_spec.rb#L20)
    * as SHIFT_JIS string
      * [gets converted to UTF-8](./lib/submodules/ably-ruby/spec/unit/rest/channel_spec.rb#L28)
      * [is compatible with original encoding](./lib/submodules/ably-ruby/spec/unit/rest/channel_spec.rb#L32)
    * as ASCII_8BIT string
      * [gets converted to UTF-8](./lib/submodules/ably-ruby/spec/unit/rest/channel_spec.rb#L40)
      * [is compatible with original encoding](./lib/submodules/ably-ruby/spec/unit/rest/channel_spec.rb#L44)
    * as Integer
      * [raises an argument error](./lib/submodules/ably-ruby/spec/unit/rest/channel_spec.rb#L52)
    * as Nil
      * [raises an argument error](./lib/submodules/ably-ruby/spec/unit/rest/channel_spec.rb#L60)
  * #publish name argument
    * as UTF_8 string
      * [is permitted](./lib/submodules/ably-ruby/spec/unit/rest/channel_spec.rb#L72)
    * as SHIFT_JIS string
      * [is permitted](./lib/submodules/ably-ruby/spec/unit/rest/channel_spec.rb#L80)
    * as ASCII_8BIT string
      * [is permitted](./lib/submodules/ably-ruby/spec/unit/rest/channel_spec.rb#L88)
    * as Integer
      * [raises an argument error](./lib/submodules/ably-ruby/spec/unit/rest/channel_spec.rb#L96)
    * as Nil
      * [raises an argument error](./lib/submodules/ably-ruby/spec/unit/rest/channel_spec.rb#L104)

### Ably::Rest::Channels
_(see [lib/submodules/ably-ruby/spec/unit/rest/channels_spec.rb](./lib/submodules/ably-ruby/spec/unit/rest/channels_spec.rb))_
  * creating channels
    * [#get creates a channel](./lib/submodules/ably-ruby/spec/unit/rest/channels_spec.rb#L12)
    * [#get will reuse the channel object](./lib/submodules/ably-ruby/spec/unit/rest/channels_spec.rb#L17)
    * [[] creates a channel](./lib/submodules/ably-ruby/spec/unit/rest/channels_spec.rb#L23)
  * #fetch
    * [retrieves a channel if it exists](./lib/submodules/ably-ruby/spec/unit/rest/channels_spec.rb#L30)
    * [calls the block if channel is missing](./lib/submodules/ably-ruby/spec/unit/rest/channels_spec.rb#L35)
  * destroying channels
    * [#release releases the channel resoures](./lib/submodules/ably-ruby/spec/unit/rest/channels_spec.rb#L43)
  * is Enumerable
    * [allows enumeration](./lib/submodules/ably-ruby/spec/unit/rest/channels_spec.rb#L59)
    * [provides #length](./lib/submodules/ably-ruby/spec/unit/rest/channels_spec.rb#L75)
    * #each
      * [returns an enumerator](./lib/submodules/ably-ruby/spec/unit/rest/channels_spec.rb#L64)
      * [yields each channel](./lib/submodules/ably-ruby/spec/unit/rest/channels_spec.rb#L68)

### Ably::Rest::Client
_(see [lib/submodules/ably-ruby/spec/unit/rest/client_spec.rb](./lib/submodules/ably-ruby/spec/unit/rest/client_spec.rb))_
  * behaves like a client initializer
    * with invalid arguments
      * empty hash
        * [raises an exception](./lib/submodules/ably-ruby/spec/shared/client_initializer_behaviour.rb#L28)
      * nil
        * [raises an exception](./lib/submodules/ably-ruby/spec/shared/client_initializer_behaviour.rb#L36)
      * api_key: "invalid"
        * [raises an exception](./lib/submodules/ably-ruby/spec/shared/client_initializer_behaviour.rb#L44)
      * api_key: "invalid:asdad"
        * [raises an exception](./lib/submodules/ably-ruby/spec/shared/client_initializer_behaviour.rb#L52)
      * api_key and key_id
        * [raises an exception](./lib/submodules/ably-ruby/spec/shared/client_initializer_behaviour.rb#L60)
      * api_key and key_secret
        * [raises an exception](./lib/submodules/ably-ruby/spec/shared/client_initializer_behaviour.rb#L68)
      * client_id as only option
        * [requires a valid key](./lib/submodules/ably-ruby/spec/shared/client_initializer_behaviour.rb#L76)
    * with valid arguments
      * api_key only
        * [connects to the Ably service](./lib/submodules/ably-ruby/spec/shared/client_initializer_behaviour.rb#L87)
      * key_id and key_secret
        * [constructs an api_key](./lib/submodules/ably-ruby/spec/shared/client_initializer_behaviour.rb#L95)
      * with a string key instead of options hash
        * [sets the api_key](./lib/submodules/ably-ruby/spec/shared/client_initializer_behaviour.rb#L103)
        * [sets the key_id](./lib/submodules/ably-ruby/spec/shared/client_initializer_behaviour.rb#L107)
        * [sets the key_secret](./lib/submodules/ably-ruby/spec/shared/client_initializer_behaviour.rb#L111)
      * with token
        * [sets the token_id](./lib/submodules/ably-ruby/spec/shared/client_initializer_behaviour.rb#L119)
      * endpoint
        * [defaults to production](./lib/submodules/ably-ruby/spec/shared/client_initializer_behaviour.rb#L125)
        * with environment option
          * [uses an alternate endpoint](./lib/submodules/ably-ruby/spec/shared/client_initializer_behaviour.rb#L132)
      * tls
        * [defaults to TLS](./lib/submodules/ably-ruby/spec/shared/client_initializer_behaviour.rb#L151)
        * set to false
          * [uses plain text](./lib/submodules/ably-ruby/spec/shared/client_initializer_behaviour.rb#L142)
          * [uses HTTP](./lib/submodules/ably-ruby/spec/shared/client_initializer_behaviour.rb#L146)
      * logger
        * default
          * [uses Ruby Logger](./lib/submodules/ably-ruby/spec/shared/client_initializer_behaviour.rb#L158)
          * [specifies Logger::ERROR log level](./lib/submodules/ably-ruby/spec/shared/client_initializer_behaviour.rb#L162)
        * with log_level :none
          * [silences all logging with a NilLogger](./lib/submodules/ably-ruby/spec/shared/client_initializer_behaviour.rb#L170)
        * with custom logger and log_level
          * [uses the custom logger](./lib/submodules/ably-ruby/spec/shared/client_initializer_behaviour.rb#L188)
          * [sets the custom log level](./lib/submodules/ably-ruby/spec/shared/client_initializer_behaviour.rb#L192)
    * delegators
      * [delegates :client_id to .auth](./lib/submodules/ably-ruby/spec/shared/client_initializer_behaviour.rb#L202)
      * [delegates :auth_options to .auth](./lib/submodules/ably-ruby/spec/shared/client_initializer_behaviour.rb#L207)
  * initializer options
    * TLS
      * disabled
        * [fails for any operation with basic auth and attempting to send an API key over a non-secure connection](./lib/submodules/ably-ruby/spec/unit/rest/client_spec.rb#L17)
    * :use_token_auth
      * set to false
        * with an api_key with :tls => false
          * [fails for any operation with basic auth and attempting to send an API key over a non-secure connection](./lib/submodules/ably-ruby/spec/unit/rest/client_spec.rb#L28)
        * without an api_key
          * [fails as an api_key is required if not using token auth](./lib/submodules/ably-ruby/spec/unit/rest/client_spec.rb#L36)
      * set to true
        * without an api_key or token_id
          * [fails as an api_key is required to issue tokens](./lib/submodules/ably-ruby/spec/unit/rest/client_spec.rb#L46)

### Ably::Rest
_(see [lib/submodules/ably-ruby/spec/unit/rest/rest_spec.rb](./lib/submodules/ably-ruby/spec/unit/rest/rest_spec.rb))_
  * [constructor returns an Ably::Rest::Client](./lib/submodules/ably-ruby/spec/unit/rest/rest_spec.rb#L7)

### Ably::Util::Crypto
_(see [lib/submodules/ably-ruby/spec/unit/util/crypto_spec.rb](./lib/submodules/ably-ruby/spec/unit/util/crypto_spec.rb))_
  * defaults
    * [match other client libraries](./lib/submodules/ably-ruby/spec/unit/util/crypto_spec.rb#L18)
  * encrypts & decrypt
    * [#encrypt encrypts a string](./lib/submodules/ably-ruby/spec/unit/util/crypto_spec.rb#L28)
    * [#decrypt decrypts a string](./lib/submodules/ably-ruby/spec/unit/util/crypto_spec.rb#L33)
  * encrypting an empty string
    * [raises an ArgumentError](./lib/submodules/ably-ruby/spec/unit/util/crypto_spec.rb#L42)
  * using shared client lib fixture data
    * with AES-128-CBC
      * behaves like an Ably encrypter and decrypter
        * text payload
          * [encrypts exactly the same binary data as other client libraries](./lib/submodules/ably-ruby/spec/unit/util/crypto_spec.rb#L65)
          * [decrypts exactly the same binary data as other client libraries](./lib/submodules/ably-ruby/spec/unit/util/crypto_spec.rb#L69)
    * with AES-256-CBC
      * behaves like an Ably encrypter and decrypter
        * text payload
          * [encrypts exactly the same binary data as other client libraries](./lib/submodules/ably-ruby/spec/unit/util/crypto_spec.rb#L65)
          * [decrypts exactly the same binary data as other client libraries](./lib/submodules/ably-ruby/spec/unit/util/crypto_spec.rb#L69)

  -------

  ## Test summary

  * Passing tests: 394
  * Pending tests: 0
  * Failing tests: 0
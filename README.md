# Symple Ruby Client

The Symple client module for Ruby which enables one to send Symple messages from a Ruby app to the server using the intermediary redis instance. 

## What is Symple?

Symple is a presence and messaging protocol which is semantically similar to XMPP, except that it is unrestrictive and vastly simplified, and it used JSON instead of XML for encoding messages. Symple also has a C++ client which makes it ideas for messaging and remoting between desktop, mobile and browser clients.

## How to use it

1. Clone the ```symple-ruby-client```
2. Simply ```include``` the file in your app
3. Call ```broadcast```, ```broadcast_user``` or ```broadcast_group``` to send a JSON serialized object.

## Examples

Here is an example of using Symple in a Rails app. Lets say we have an Event model, and we want to broadcast the serialized object to all connected peers in the user group when a new event is created:

    class Event < ActiveRecord::Base
      belongs_to :user      
      after_create :symple_broadcast
      
      def symple_broadcast
        Symple.broadcast_group(self.user.group_id, {
            name: self.name,                                     # name of the event
            type: self.class.name,                               # the name of the object being serialized
            from: "#{self.user.username}@#{self.user.group_id}", # sender symple address
            data: self                                           # pass the instance to be serialized using to_json
          })
      end
    end

## Contributing

1. Fork it
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am 'Add some feature'`)
4. Push to the branch (`git push origin my-new-feature`)
5. Create new Pull Request

## Contact

For more information on Symple please check out http://sourcey.com/symple/
For the C++ client see https://github.com/sourcey/libsourcey/tree/master/src/symple

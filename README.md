# Tweet Now! 1: Single-User

## Learning Competencies

* Incorporate third-party gems into a web application using bundler
* Extend the Sinatra application environment with a ruby gem (OAuth)
* Use a third-party API
* Write custom event handlers in JavaScript and jQuery
* Implement synchronous / asynchronous requests in a web application


## Summary

This is the first challenge in a sequence of challenges you'll come upon next week, leading up to building an application to schedule future tweets and deploying it to Heroku.

To start, we'll create a command line application that lets a single user tweet. We'll hard-code the user from which we'll be tweeting. [Later we'll add support for logging in via Twitter](../../../tweet-now-2-multi-user-challenge) so that anyone can tweet. After that, we'll add the ability to [schedule tweets into the future](../../../tweet-later-challenge).

You'll learn about background jobs, tasks queues, and scheduled tasks over these challenges &mdash; three fundamental components to building a usable, scalable web application.  You'll also learn some sysadmin voodoo to manage development and production credentials, and a bit of the inner workings of OAuth.

All in a day's work, right?

## Releases

### Release 0 - Tweet It!

Create an interactive command line application that tweets (actually sends and posts the Tweet on Twitter) whatever text the user inputs. 

The account from which the application tweets should be hard-coded for now. You can make it your own personal tweet page if you'd like.

We will be using similar code as yesterday, except we will be using `Net::HTTP` to make `POST` requests. Here's some code for doing just that (you can find this in the skeleton in `/lib/oauth_client.rb`):

```ruby
require 'simple_oauth'

class OAuthClient
  attr_reader :credentials

  def initialize(credentials)
    raise ArgumentError, "must provide consumer_key, consumer_secret, token, and token_secret" unless valid_credentials?(credentials)
    @credentials = credentials
  end

  def post(url)
    # create the HTTP post request

    # request =  ** fill this in
    #hint - this request is going to need some form data (aka your tweet)

    # set the Authorization Header using the oauth helper
    request["Authorization"] = oauth_header(request)

    # connect to the server and send the request
    # response = ** fill this in

    return response
  end

  private

  # A helper method to generate the OAuth Authorization header given
  # an Net::HTTP::GenericRequest object and a Hash of params
  def oauth_header(request)
    SimpleOAuth::Header.new(request.method, request.uri, URI.decode_www_form(request.body), credentials).to_s
  end

  def valid_credentials?(credentials)
    [:consumer_key, :consumer_secret, :token, :token_secret].all? { |key| credentials[key] }
  end
end

# sample usage:

client = OAuthClient.new(
    consumer_key: "YOUR TWITTER APP'S API KEY",
    consumer_secret: "YOU TWITTER APP'S API SECRET",
    token: "YOUR ACCESS TOKEN",
    token_secret: "YOUR ACCESS TOKEN SECRET"
)

p client.post("https://api.twitter.com/1.1/statuses/update.json").body

```

You'll need to fill in the `#post` method so that it uses the `url` to make `POST` requests to the Twitter API.

Note: Be sure to research how you are going to attach params to your `request`.

### Release 1 

Now, combine the ability to post tweets and get the most recent tweets from a user (aka the previous challenge). After a user tweets, he or she should be able to view an updated 'twitter feed'.


## Resources

* [jQuery `.on()` Documentation](http://api.jquery.com/on/) - intercept browser events with jQuery
* [Enabling / Disabling Elements With jQuery](http://learn.jquery.com/using-jquery-core/faq/how-do-i-disable-enable-a-form-element/)
* [Tweet Now! 2: Multi-User Challenge](../../../tweet-now-2-multi-user-challenge)
* [Tweet Later Challenge](../../../tweet-later-challenge)

[jQuery `.on()` Documentation]:http://api.jquery.com/on/
[Enabling / Disabling Elements With jQuery]:http://jquery-howto.blogspot.com/2008/12/how-to-disableenable-element-with.html
[Tweet Now! 2: Multi-User Challenge]:https://github.com/Devbootcamp/tweet-now-2-multi-user-challenge
[Tweet Later Challenge]:https://github.com/Devbootcamp/tweet-later-challenge

# Sending emails and text messages

## Example of how to send an email with the Mailgun gem

In your `Gemfile`, add:

```ruby
gem "mailgun-ruby"
```

Then, at a Terminal prompt:

```bash
bundle install
```

You will then have access to the `Mailgun::Client` class. It is used like this:

```ruby
# Retrieve your credentials from secure storage
mg_api_key = ENV.fetch("MAILGUN_API_KEY")
mg_sending_domain = ENV.fetch("MAILGUN_SENDING_DOMAIN")

# Create an instance of the Mailgun Client and authenticate with your API key
mg_client = Mailgun::Client.new(mg_api_key)

# Craft your email as a Hash with these four keys
email_parameters =  { 
  :from => "umbrella@appdevproject.com",
  :to => "put-your-own-email-address-here@example.com",  # Put your own email address here if you want to see it in action
  :subject => "Take an umbrella today!",
  :text => "It's going to rain today, take an umbrella with you!"
}

# Send your email!
mg_client.send_message(mg_sending_domain, email_parameters)
```

## Example of how to send an SMS with the Twilio gem

In your `Gemfile`, add:

```ruby
gem "twilio-ruby"
```

Then, at a Terminal prompt:

```bash
bundle install
```

You will then have access to the `Twilio::REST::Client` class. It is used like this:

```ruby
# Retrieve your credentials from secure storage
twilio_sid = ENV.fetch("TWILIO_ACCOUNT_SID")
twilio_token = ENV.fetch("TWILIO_AUTH_TOKEN")
twilio_sending_number = ENV.fetch("TWILIO_SENDING_PHONE_NUMBER")

# Create an instance of the Twilio Client and authenticate with your API key
twilio_client = Twilio::REST::Client.new(twilio_sid, twilio_token)

# Craft your SMS as a Hash with three keys
sms_parameters = {
  :from => twilio_sending_number,
  :to => "+19876543210", # Put your own phone number here if you want to see it in action
  :body => "It's going to rain today — take an umbrella!"
}

# Send your SMS!
twilio_client.api.account.messages.create(sms_parameters)
```

 - Sign up for your own Twilio account — [if use this referral link you'll $10 in credit](https://www.twilio.com/referral/86ykDX), and so will our class account.
 - [Twilio Ruby Quickstarts](https://www.twilio.com/docs/quickstart/ruby)

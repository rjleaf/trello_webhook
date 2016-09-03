[![Build Status](https://travis-ci.org/ssaunier/trello_webhook.svg?branch=master)](https://travis-ci.org/ssaunier/trello_webhook)
[![Gem Version](https://badge.fury.io/rb/trello_webhook.svg)](http://badge.fury.io/rb/trello_webhook)


# TrelloWebhook

This gem will help you to quickly setup a route in your Rails application which listens
to a [Trello webhook](https://trello.com/docs/api/webhook/index.html)

## Installation

Add this line to your application's Gemfile:

```ruby
gem 'trello_webhook'
```

And then execute:

```bash
bundle install
```

## Configuration

First, configure a route to receive the trello webhook POST requests.

```ruby
# config/routes.rb
resource :trello_webhooks, only: %i(show create), defaults: { formats: :json }
```

Then create a new controller:

```ruby
# app/controllers/trello_webhooks_controller.rb
class TrelloWebhooksController < ActionController::Base
  include TrelloWebhook::Processor

  def update_card(payload)
    # TODO: handle updateCard webhook payload
  end

  def webhook_secret
    ENV['TRELLO_DEVELOPER_SECRET']  # From https://trello.com/app-key
  end
end
```

Add as many instance methods as events you want to handle in your controller.

## Adding the webhook to a given board

You need the `ruby-trello` gem.

```ruby
webhook = Trello::Webhook.new
webhook.description = "The webhook description"
webhook.id_model = "The board model id" # Run `Trello::Board.all` to find it.
webhook.callback_url = "#{ENV['BASE_URL']}/trello_webhooks"  # BASE_URL is your website's url. Use ngrok in dev.
webhook.save
```

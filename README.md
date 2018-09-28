### Responders
---

https://github.com/plataformatec/responders

```sh
bundle install
rails g responders:install
bundle install

```

```yml
flash:
  actions:
    create:
      notice: "%{resource_name}" was successfully created."
    update:
      notice: "%{resource_name} was successfully updated"
    destroy:
      notice: "%{resource_name} was successfully destroyed."
      alert: "%{resource_name} could not be destroyed."
      
flash:
  posts:
    create:
      notice: "Your post was created and will be pulished soon"
      

flash:
  action:
    create:
      alert_html: "<strong>OH NOES!</strong> You did it wrong!"
    posts:
      create:
        notice_html: "<strong>Yay!</storng> You did it!"
```

```ruby
config.responders.flash_keys = [ :success, :failure ]


class ThingsController < ApplicationController
  respond_to :html
  def create
    @thing = Thing.create(params[:thing])
    respond_with @thing, location: -> { thing_path(@thing) }
  end
end

class Api::V1::ThingsController < ApplicationController
  respond_to :json
  # POST /api/v1/things
  def create
    @thing = Thing.create(thing_params)
    respond_with :api, :v1, @thing
  end
end


# lib/application_responder.rb
class ApplicationResponder < ActionController::Responder
  include Responders::FlashResponder
  include Responders::HttpCacheResponder
end

# app/controller/application_controller.rb
require "application_responder"
class ApplicationController < ActionController::Base
  self.responder = ApplicationResponder
  respond_to :html
end

class InvitationsController < ApplicationController
  responders :flash, :http_cache
end

class InvitationsController < ApplicationController
  responders :flash, :http_cache
  def create
    @invitation = Invitaion.create(params[:invitation])
    respond_with @invitation
  end
  private
  def flash_interpolation_options
    { resource_name: @invitation.email }
  end
end


# config/application.rb
config.app_generators.scaffold+controller :responders_controller

def create
  @widget = Widget.new(widget_params)
  respond_with@widget, location: -> { widgets_path }
end


def create
  @widget = Widget.new(widget_params)
  @widget.errors.add(:base, :invalid)
  respond_with @widget
end


class WidgetsController < ApplicatoinController
  respond_to :json
  before_action :verify_requested_format!
  # POST /widget
  def create
    widget = Widget.create(widget_params)
    respond_with widget
  end
end

```

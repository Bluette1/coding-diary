---
title: Rails With Devise Api Gem
date: "2024-08-29T22:40:32.169Z"
description: How to Build a Rails App with Devise Api.
---

### Installing Gems
Assuming you have an existing Rails API, run the following commands in the terminal:

```bash
bundle add devise devise-api

rails g devise:install && rails g devise user

rails g devise_api:install

```

### Configure Cors
```bash
# Gemfile
gem 'rack-cors'

```
Modify `cors.rb`

```bash
# config/initializers/cors.rb

# Be sure to restart your server when you modify this file.

# Avoid CORS issues when API is called from the frontend app.
# Handle Cross-Origin Resource Sharing (CORS) in order to accept cross-origin Ajax requests.

# Read more: https://github.com/cyu/rack-cors

Rails.application.config.middleware.insert_before 0, Rack::Cors do
  allow do
    origins "*"

    resource "*",
      headers: :any,
      methods: [:get, :post, :put, :patch, :delete, :options, :head]
  end
end

```

### Create Controller class as follows
```bash
class ImagesController < ApplicationController
  before_action :authenticate_devise_api_token!, only: [:create]

  def index; end

  def create
    devise_api_token = current_devise_api_token

    if devise_api_token
      render json: { message: "You are logged in as #{devise_api_token.resource_owner.email}" }, status: :ok
    else
      render json: { message: 'You are not logged in' }, status: :unauthorized
    end
  end
end
```

### Modify the User Model

```bash
class User < ApplicationRecord
  # Include default devise modules. Others available are:
  # :confirmable, :lockable, :timeoutable, :trackable and :omniauthable
  devise :database_authenticatable, :registerable,
         :recoverable, :rememberable, :validatable, :api #add this
end

```

### Testing
Hit the following routes using Postman:

Signup:
```

POST http://localhost:3000/users/tokens/sign_up
Content-Type: application/json

{
    "email": "mary.sawyer@gmail.com",
    "password": "password123&"

}

```

Login:

```
POST http://localhost:3000/users/tokens/sign_in
Content-Type: application/json

{
    "email": "mary.sawyer@gmail.com",
    "password": "password123&"

}

```

Protected Route:

```

POST http://localhost:3000/images
Content-Type: application/json
Authorization: Bearer QL4sqV4Q7-yZcAKvmaxVCYqsBaHwpw81Jks2sk5mKjPiijxG5jJsuki7JBtU

{
    "image": "image"
    
}

```

For further information regarding the available authentication routes: see[Devise Api documentation](https://github.com/nejdetkadir/devise-api)

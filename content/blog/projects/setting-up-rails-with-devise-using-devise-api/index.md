---
title: Setting Up Rails With Devise Using the Devise Api Gem
date: "2024-09-18T22:40:32.169Z"
description: Set Up Rails With Devise Using the Devise Api Gem.
---

Let’s walk through the entire process of setting up Devise with the `devise-api` gem in a Rails API application using Mongoid. 

### Step-by-Step Setup

#### 1. Create a New Rails API Application

If you haven’t already created your Rails API application, you can do so with the following command:

```bash
rails new my_api --api
cd my_api
```

#### 2. Add Required Gems

In your `Gemfile`, add the following gems:

```ruby
gem 'mongoid', '~> 7.0'
gem 'devise'
gem 'devise-api'
```

Then run:

```bash
bundle install
```

#### 3. Configure Mongoid

Run the Mongoid generator to create the configuration file:

```bash
rails g mongoid:config
```

This will create `config/mongoid.yml`, where you can configure your MongoDB settings.

#### 4. Set Up Devise

Run the Devise installation generator:

```bash
rails generate devise:install
```

Follow any additional instructions displayed in the terminal (like adding a root route).

#### 5. Generate User Model

Create a User model that will use Mongoid. You can manually create it, as shown below:

```ruby
# app/models/user.rb
class User
  include Mongoid::Document
  include Devise::Models

  field :email, type: String
  field :encrypted_password, type: String

  # Add additional fields as necessary

  devise :database_authenticatable, :registerable,
         :recoverable, :rememberable, :validatable, :api

  has_many :tokens, class_name: 'Token'

  def generate_access_token
    token = SecureRandom.hex(10)
    tokens.create(access_token: token, expires_at: 1.hour.from_now)
    token
  end

  def invalidate_tokens
    tokens.delete_all
  end
end
```

**Note:** You need to add :api module to your devise model. For example:
```
class User
  include Mongoid::Document
  include Devise::Models

  field :email, type: String
  field :encrypted_password, type: String

  # Add additional fields as necessary

  devise :database_authenticatable, :registerable,
         :recoverable, :rememberable, :validatable, :api
```

#### 6. Create Token Model

Next, create the Token model manually:

```ruby
# app/models/token.rb
class Token
  include Mongoid::Document

  field :access_token, type: String
  field :expires_at, type: DateTime

  belongs_to :user
end
```

#### 7. Create Custom Controllers

Create custom controllers to manage user sessions and registrations:

- **Sessions Controller**:

```ruby
# app/controllers/users/sessions_controller.rb
class Users::SessionsController < Devise::SessionsController
  respond_to :json

  def create
    self.resource = warden.authenticate!(auth_options)
    sign_in(resource_name, resource)

    access_token = resource.generate_access_token

    render json: { user: resource, access_token: access_token }, status: :created
  rescue StandardError
    render json: { errors: ['Invalid credentials'] }, status: :unauthorized
  end

  def destroy
    resource = User.find_by(email: params[:user][:email])
    resource.invalidate_tokens if resource
    sign_out(resource_name)

    render json: { message: 'Logged out' }, status: :ok
  end
end
```

- **Registrations Controller**:

```ruby
# app/controllers/users/registrations_controller.rb
class Users::RegistrationsController < Devise::RegistrationsController
  respond_to :json

  def create
    build_resource

    if resource.save
      yield resource if block_given?
      render json: resource, status: :created
    else
      render json: { errors: resource.errors.full_messages }, status: :unprocessable_entity
    end
  end
end
```

#### 8. Configure Routes

Set up your routes in `config/routes.rb`:

```ruby
Rails.application.routes.draw do
  devise_for :users, controllers: {
    sessions: 'users/sessions',
    registrations: 'users/registrations'
  }
end
```

#### 9. Configure Devise for API

In `config/initializers/devise.rb`, set `navigational_formats` to avoid rendering HTML responses:

```ruby
Devise.setup do |config|
  config.navigational_formats = [] # Disable HTML responses
end
```

#### 10. Start Your Rails Server

Now that everything is set up, start your server:

```bash
rails s
```

#### 11. Test the Endpoints

You can use tools like Postman or cURL to test the following endpoints:

- **Sign Up**:
  ```http
  POST /users
  Content-Type: application/json

  {
    "user": {
      "email": "user@example.com",
      "password": "password123",
      "password_confirmation": "password123"
    }
  }
  ```

- **Sign In**:
  ```http
  POST /users/sign_in
  Content-Type: application/json

  {
    "user": {
      "email": "user@example.com",
      "password": "password123"
    }
  }
  ```

- **Sign Out**:
  ```http
  DELETE /users/sign_out
  Content-Type: application/json

  {
    "user": {
      "email": "user@example.com"
    }
  }
  ```

### Additional Features

You can enhance your setup by adding password recovery, email confirmation, and other Devise features as needed.


#### Adding Refresh Tokens
The error message you’re seeing indicates that there is a conflict with the route names generated by Devise. This can happen if you’re trying to create a new resource with the same name as an existing route, particularly if you already have routes set up for `devise_for :users`.

### Here’s How to Resolve It:

1. **Check Existing Routes**:
   Run the following command to see your current routes:

   ```bash
   rails routes
   ```

   Look for routes related to `user_sessions`. You might find that the route `new_user_session` already exists.

2. **Avoid Conflicting Model Generation**:
   Since you're using Mongoid and don’t need ActiveRecord migrations, you don’t actually need to generate a migration for the `Token` model. Instead, you can just create the model file manually.

### Create the Token Model Manually

1. **Create the Model File**:
   Instead of running the generator, create the model manually:

   Create a file at `app/models/token.rb`:

   ```ruby
   # app/models/token.rb
   class Token
     include Mongoid::Document

     field :access_token, type: String
     field :refresh_token, type: String
     field :expires_at, type: DateTime

     belongs_to :user

     def self.generate_tokens(user)
       access_token = SecureRandom.hex(10)
       refresh_token = SecureRandom.hex(10)
       expires_at = 1.hour.from_now

       user.tokens.create(
         access_token: access_token,
         refresh_token: refresh_token,
         expires_at: expires_at
       )

       { access_token: access_token, refresh_token: refresh_token }
     end
   end
   ```

2. **Update the User Model**:
   Add a one-to-many relationship in your `User` model to link to tokens.

   ```ruby
   # app/models/user.rb
   class User
     include Mongoid::Document
     include Devise::Models

     field :email, type: String
     field :encrypted_password, type: String

     has_many :tokens, class_name: 'Token'

     devise :database_authenticatable, :registerable,
            :recoverable, :rememberable, :validatable, :api

     # Include methods for token management here...
   end
   ```

3. **Restart Your Server**:
   If your server is running, restart it to ensure all changes are loaded.

### Summary

By manually creating the `Token` model, you avoid any potential routing conflicts while still achieving the desired functionality. 

### 422 Unprocessable Entity
The `422 Unprocessable Entity` response indicates that something went wrong with the user registration process. This often means that the request is not valid due to missing or incorrect data. Let's troubleshoot the issue step by step.

### Steps to Diagnose and Fix the Issue

1. **Check Parameters**:
   The log indicates that the parameters might be nested incorrectly. Specifically, it seems like you're sending a `registration` key. Make sure you're sending the parameters correctly.

   The JSON payload should look like this:

   ```json
   {
     "user": {
       "email": "job.sacha@gmail.com",
       "password": "yourpassword",
       "password_confirmation": "yourpassword"
     }
   }
   ```

   Ensure your client (Postman or cURL) is sending the request in this format.

2. **Modify the Registrations Controller**:
   Update the `Users::RegistrationsController` to handle potential parameter issues:

   ```ruby
   # app/controllers/users/registrations_controller.rb
   class Users::RegistrationsController < Devise::RegistrationsController
     respond_to :json

     def create
       build_resource(sign_up_params)

       if resource.save
         yield resource if block_given?
         render json: resource, status: :created
       else
         render json: { errors: resource.errors.full_messages }, status: :unprocessable_entity
       end
     end

     private

     def sign_up_params
       params.require(:user).permit(:email, :password, :password_confirmation)
     end
   end
   ```

   The `sign_up_params` method ensures that you’re permitting the right parameters, which can help avoid issues.

3. **Check User Model Validations**:
   If your `User` model has any custom validations, they might be causing the save to fail. For instance, if you have a uniqueness validation on the email field, trying to register with an already taken email will result in a 422 response.

   To check this, look at your `User` model:

   ```ruby
   # app/models/user.rb
   class User
     include Mongoid::Document
     include Devise::Models

     field :email, type: String
     field :encrypted_password, type: String

     validates :email, presence: true, uniqueness: true
     validates :encrypted_password, presence: true

     # ... other methods ...
   end
   ```

4. **View Error Messages**:
   The error messages returned in the response can provide insight into what went wrong. Ensure you log or inspect these messages. The updated `RegistrationsController` will include any validation errors in the response.

5. **Test Again**:
   After making the changes, test the sign-up endpoint again with the correct JSON format:

   ```http
   POST /users
   Content-Type: application/json

   {
     "user": {
       "email": "job.sacha@gmail.com",
       "password": "password123",
       "password_confirmation": "password123"
     }
   }
   ```

### Conclusion

By following these steps, you should be able to resolve the 422 error and successfully register users.



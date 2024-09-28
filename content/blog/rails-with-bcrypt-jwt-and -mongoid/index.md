---
title: Rails With Bcrypt, JWT and Mongoid
date: "2024-09-28T22:40:32.169Z"
description: How to Set up Rails Authentication with Bcrypt, JWT and Mongoid.
---

If you're encountering issues with `devise-mongoid` and Rails 7, you're not alone—some gems have lagged behind the latest Rails versions. Here are a few alternative approaches you can consider for authentication with MongoDB in Rails 7:

### 1. **Use Mongoid with Custom Authentication**

You can implement your own authentication system using Mongoid without Devise. This involves creating user models, handling password hashing (using `bcrypt`), and managing sessions or tokens for authentication.

### 2. **Auth0 or Firebase Authentication**

Consider using external authentication services like Auth0 or Firebase. These services handle user authentication and can be integrated with your Rails app easily.

### 3. **Use `Sorcery`**

`Sorcery` is a flexible authentication library that can be used with MongoDB by creating a custom adapter. While it requires a bit more setup, it offers more control over the authentication process.

### 4. **Community Gems**

Look for community-maintained gems or forks that have updated support for Rails 7 and MongoDB. Check repositories on GitHub or RubyGems for newer solutions.

### 5. **Explore Other ORMs**

If you’re open to switching, you might consider using PostgreSQL or MySQL with ActiveRecord, where Devise works seamlessly. If you prefer document stores, sticking with MongoDB could require more manual handling of authentication.

### Example of Custom Authentication

Here’s a simple outline for creating a user model and authentication without Devise:

1. Installing gems
   Assuming you have an existing Rails API, run the following commands in the terminal:

```bash
bundle add mongoid bcrypt jwt
rails g mongoid:config

```

1. **Create User Model with Mongoid:**

   ```ruby
   class User
     include Mongoid::Document
     include ActiveModel::SecurePassword

     field :email, type: String
     field :password_digest, type: String

     has_secure_password
   end
   ```

2. **Register and Authenticate Users:**

   ```ruby
   class Users::RegistrationsController < ApplicationController
   def create
    user = User.new(user_params)
    if user.save
      render json: { message: 'User created' }, status: :created
    else
      render json: { errors: user.errors.full_messages }, status: :unprocessable_entity
    end
   end


   private

    def user_params
      params.require(:user).permit(:email, :password)
    end
   end
   
   ```

```ruby
class Users::SessionsController < ApplicationController
  def login
    user = User.find_by(email: params[:email])
    if user&.authenticate(params[:password])
      token = generate_jwt(user)
      render json: { token: }, status: :created
    else
      render json: { error: 'Invalid credentials' }, status: :unauthorized
    end
  end

  # Generate a JWT token for authentication

  def generate_jwt(user)
    payload = { user_id: user.id, exp: 24.hours.from_now.to_i }
    JWT.encode(payload, Rails.application.credentials.jwt_secret_key)
  end

end

```

### Generate a JWT token for authentication



**Decode and Validate the JWT Token:**
   Create a method to decode the token and verify the user's identity.

   ```ruby
   def authenticate_user!
     token = request.headers['Authorization']&.split(' ')&.last
     begin
       decoded = JWT.decode(token, Rails.application.credentials.jwt_secret_key, true, { algorithm: 'HS256' })
       @current_user = User.find(decoded[0]['user_id'])
     rescue JWT::DecodeError
       render json: { error: 'Invalid token' }, status: :unauthorized
     end
   end
   ```

**Logout:**
   Token-based systems usually don’t require a logout endpoint, but you can implement a token revocation strategy if needed.

### Choosing Between Sessions and JWT

- **Sessions** are simpler and work well for server-rendered applications where you can maintain state easily.
- **JWT** is great for stateless APIs, especially when you want to enable cross-domain authentication or mobile app access.

To implement a token-based logout endpoint, you typically need a way to invalidate the token. This can be achieved in a few different ways, depending on your application's requirements. Here’s a common approach:

### 1. **Token Revocation Strategy**

You can maintain a blacklist of revoked tokens or simply set an expiration time for the tokens and rely on that.

### Example Implementation

1. **Create a Revoked Token List (Optional)**:
   If you choose to maintain a blacklist, create a model to store revoked tokens.

   ```ruby
   class RevokedToken
     include Mongoid::Document
     
     field :token, type: String
     field :revoked_at, type: Time
   end
   ```

2. **Logout Endpoint**:
   In your controller, create a logout method to handle the revocation of the token.

   ```ruby
   class UsersController < ApplicationController
     # Existing login method...

     def logout
       token = request.headers['Authorization']&.split(' ')&.last

       if token.present?
         # Option 1: Blacklist the token
         RevokedToken.create(token: token, revoked_at: Time.current)
         
         # Option 2: Just inform the user (relying on token expiration)
         render json: { message: 'Logged out successfully' }, status: :ok
       else
         render json: { error: 'Token not provided' }, status: :unprocessable_entity
       end
     end
   end
   ```

3. **Authenticate User with Revocation Check**:
   Modify your authentication method to check against the revoked tokens.

   ```ruby
   def authenticate_user!
     token = request.headers['Authorization']&.split(' ')&.last
     return render json: { error: 'Token not provided' }, status: :unauthorized unless token

     # Check if the token is revoked
     if RevokedToken.where(token: token).exists?
       return render json: { error: 'Token has been revoked' }, status: :unauthorized
     end

     begin
       decoded = JWT.decode(token, Rails.application.credentials.jwt_secret_key, true, { algorithm: 'HS256' })
       @current_user = User.find(decoded[0]['user_id'])
     rescue JWT::DecodeError
       render json: { error: 'Invalid token' }, status: :unauthorized
     end
   end
   ```

### Advantages of This Approach

- **Security**: By blacklisting tokens, you ensure that any compromised tokens can be invalidated immediately.
- **Flexibility**: You can add more attributes to the `RevokedToken` model if you want to track additional information (like user ID or reason for revocation).

### Considerations

- **Performance**: Storing and checking revoked tokens may introduce some overhead. Ensure you have appropriate indexing on the database.
- **Cleanup**: Implement a mechanism to periodically clean up old revoked tokens to prevent the database from growing indefinitely.

You can view the actual code [here](https://github.com/Bluette1/rails-with-bcrypt-jwt-and-mongoid)


---
title: Bookshare App Using Hotwire & Stimulus
date: "2024-09-18T22:40:32.169Z"
description: Build a Bookshare App Using Hotwire & Stimulus.
---
Here’s a step-by-step guide to start a Rails app project called **Book Share** that uses Hotwire and Stimulus for user accounts and sharing what users are reading.

### Step 1: Set Up Your Rails Environment

1. **Install Rails** (if you haven't already):
   ```bash
   gem install rails
   ```

2. **Create a new Rails app**:
   ```bash
   rails new book-share --database=postgresql
   cd book-share
   ```

3. **Add Hotwire**:
   Hotwire is included by default in Rails 7+. If you're using an earlier version, you can add it manually:
   ```bash
   bundle add hotwire-rails
   ```

4. **Install Stimulus**:
   It should be installed with Hotwire, but make sure to run:
   ```bash
   bin/rails hotwire:install
   ```

5. **Set Up the Database**:
   Create the database:
   ```bash
   rails db:create
   ```

### Step 2: Generate User Authentication

You can use Devise for user authentication:

1. **Add Devise to your Gemfile**:
   ```ruby
   gem 'devise'
   ```

2. **Bundle Install**:
   ```bash
   bundle install
   ```

3. **Install Devise**:
   ```bash
   rails generate devise:install
   ```

4. **Generate the User model**:
   ```bash
   rails generate devise User
   ```

5. **Run Migrations**:
   ```bash
   rails db:migrate
   ```

### Step 3: Create the Book Model

1. **Generate the Book model**:
   ```bash
   rails generate model Book title:string author:string user:references
   ```

2. **Run Migrations**:
   ```bash
   rails db:migrate
   ```

### Step 4: Set Up Associations

In `app/models/user.rb`:
```ruby
class User < ApplicationRecord
  # Add Devise modules here
  has_many :books
end
```

In `app/models/book.rb`:
```ruby
class Book < ApplicationRecord
  belongs_to :user
end
```

### Step 5: Create Controllers and Views

1. **Generate Books Controller**:
   ```bash
   rails generate controller Books index create new
   ```

2. **Set Up Routes**:
   In `config/routes.rb`:
   ```ruby
   Rails.application.routes.draw do
     devise_for :users
     resources :books, only: [:index, :create, :new]
     root 'books#index'
   end
   ```

3. **Create Views**:
   - In `app/views/books/index.html.erb`, display shared books:
     ```erb
     <h1>Books Shared</h1>
     <%= link_to 'Share a Book', new_book_path %>
     <div id="books">
       <%= render @books %>
     </div>
     ```

   - In `app/views/books/_book.html.erb`, show individual book details:
     ```erb
     <div>
       <h2><%= book.title %> by <%= book.author %></h2>
       <p>Shared by <%= book.user.email %></p>
     </div>
     ```

   - In `app/views/books/new.html.erb`, create a form to share a book:
     ```erb
     <h1>Share a Book</h1>
     <%= form_with(model: @book, local: false) do |form| %>
       <%= form.label :title %>
       <%= form.text_field :title %>

       <%= form.label :author %>
       <%= form.text_field :author %>

       <%= form.submit 'Share' %>
     <% end %>
     ```

### Step 6: Add Turbo and Stimulus Functionality

1. **Update Books Controller**:
   In `app/controllers/books_controller.rb`:
   ```ruby
   class BooksController < ApplicationController
     before_action :authenticate_user!, only: [:new, :create]

     def index
       @books = Book.includes(:user).all
     end

     def new
       @book = Book.new
     end

     def create
       @book = current_user.books.new(book_params)
       if @book.save
         respond_to do |format|
           format.turbo_stream
           format.html { redirect_to books_path, notice: 'Book shared successfully.' }
         end
       else
         render :new
       end
     end

     private

     def book_params
       params.require(:book).permit(:title, :author)
     end
   end
   ```

2. **Add Turbo Stream Response**:
   In `app/views/books/create.turbo_stream.erb`:
   ```erb
   <%= turbo_stream.prepend "books", partial: "books/book", locals: { book: @book } %>
   ```

### Step 7: Final Touches

- Ensure you have the necessary styles and JavaScript set up.
- Test the application locally using:
  ```bash
  bin/rails server
  ```

### Conclusion

Now you have a basic **Book Share** application where users can create accounts, share what they are reading, and connect. You can further expand the app with features like user profiles, comments, or a search function as you continue to develop your skills!

### Integrating with Mobile Tech
Integrating your **Book Share** app with mobile technology can enhance user experience and accessibility. Here are several approaches to consider:

### 1. Responsive Design

- **CSS Frameworks**: Use frameworks like Bootstrap or Tailwind CSS to ensure your app is mobile-friendly. This allows your web application to adapt to different screen sizes.

### 2. Progressive Web App (PWA)

- **PWA Features**: Convert your Rails app into a PWA, enabling features like offline access, push notifications, and the ability to install the app on mobile devices.
  - Use the `pwa-rails` gem or implement service workers manually to cache resources and handle offline functionality.

### 3. API Development

- **Create a RESTful API**: Build an API for your Rails app to allow mobile applications (iOS/Android) to interact with the backend.
  - Use `ActiveModel::Serializers` or `Jbuilder` to format your JSON responses.
  
- **Mobile App Development**: Develop native or cross-platform mobile apps using frameworks like:
  - **React Native**: Create a mobile app that consumes your Rails API.
  - **Flutter**: Build a cross-platform app with a single codebase.

### 4. Native Mobile Integration

- **Web Views**: If you have existing mobile apps, you can embed your Rails app in a web view, allowing users to access the Book Share functionality directly within the app.
  
- **Deep Linking**: Implement deep linking to allow users to open specific content or pages in your app directly from notifications or other apps.

### 5. User Authentication

- **OAuth**: Implement OAuth for authentication to allow users to sign in with social media accounts (like Google or Facebook) on mobile devices easily.

### 6. Push Notifications

- **Integrate with Services**: Use services like Firebase Cloud Messaging (FCM) to send push notifications for new book shares, comments, or interactions to users on their mobile devices.

### 7. Analytics and Monitoring

- **Integrate Analytics**: Use tools like Google Analytics or Mixpanel to track user interactions and engagement within your mobile app, helping you improve user experience.

### 8. Continuous Integration and Deployment

- **CI/CD for Mobile**: Set up a CI/CD pipeline to automate testing and deployment for your mobile applications alongside your Rails backend.

### Conclusion

By implementing these strategies, you can effectively integrate your **Book Share** app with mobile technology, providing users with a seamless experience across different devices. Whether you choose to enhance the web app's responsiveness or develop a standalone mobile app, the key is to focus on usability and accessibility.

### Build a React Native App to Consume the book-share API
Creating a React Native app to consume the **Book Share** API involves several steps. Here’s a step-by-step guide to get you started:

### Step 1: Set Up Your React Native Environment

1. **Install Node.js**: Ensure you have Node.js installed. You can download it from [nodejs.org](https://nodejs.org/).

2. **Install React Native CLI**:
   ```bash
   npm install -g react-native-cli
   ```

3. **Create a New React Native Project**:
   ```bash
   npx react-native init BookShareApp
   cd BookShareApp
   ```

### Step 2: Install Required Dependencies

You will need some libraries for navigation and making HTTP requests.

1. **React Navigation**:
   ```bash
   npm install @react-navigation/native
   npm install @react-navigation/native-stack
   npm install react-native-gesture-handler react-native-reanimated react-native-screens react-native-safe-area-context @react-native-community/masked-view
   ```

2. **Axios** (for making API calls):
   ```bash
   npm install axios
   ```

### Step 3: Set Up Navigation

1. **Create a Navigation Container**:
   In your `App.js`, set up the navigation:
   ```javascript
   import * as React from 'react';
   import { NavigationContainer } from '@react-navigation/native';
   import { createNativeStackNavigator } from '@react-navigation/native-stack';
   import HomeScreen from './screens/HomeScreen';
   import ShareBookScreen from './screens/ShareBookScreen';

   const Stack = createNativeStackNavigator();

   export default function App() {
     return (
       <NavigationContainer>
         <Stack.Navigator initialRouteName="Home">
           <Stack.Screen name="Home" component={HomeScreen} />
           <Stack.Screen name="ShareBook" component={ShareBookScreen} />
         </Stack.Navigator>
       </NavigationContainer>
     );
   }
   ```

### Step 4: Create Screens

1. **Create a Home Screen**:
   In `screens/HomeScreen.js`, fetch and display the shared books:
   ```javascript
   import React, { useEffect, useState } from 'react';
   import { View, Text, Button, FlatList } from 'react-native';
   import axios from 'axios';

   const HomeScreen = ({ navigation }) => {
     const [books, setBooks] = useState([]);

     useEffect(() => {
       const fetchBooks = async () => {
         const response = await axios.get('http://your-api-url.com/books');
         setBooks(response.data);
       };
       fetchBooks();
     }, []);

     return (
       <View>
         <Button title="Share a Book" onPress={() => navigation.navigate('ShareBook')} />
         <FlatList
           data={books}
           keyExtractor={(item) => item.id.toString()}
           renderItem={({ item }) => (
             <View>
               <Text>{item.title} by {item.author}</Text>
             </View>
           )}
         />
       </View>
     );
   };

   export default HomeScreen;
   ```

2. **Create a Share Book Screen**:
   In `screens/ShareBookScreen.js`, create a form to share a book:
   ```javascript
   import React, { useState } from 'react';
   import { View, TextInput, Button, Alert } from 'react-native';
   import axios from 'axios';

   const ShareBookScreen = ({ navigation }) => {
     const [title, setTitle] = useState('');
     const [author, setAuthor] = useState('');

     const shareBook = async () => {
       try {
         await axios.post('http://your-api-url.com/books', {
           title,
           author,
           user_id: 1 // Replace with actual user ID
         });
         Alert.alert('Book shared successfully!');
         navigation.goBack();
       } catch (error) {
         Alert.alert('Error sharing book:', error.message);
       }
     };

     return (
       <View>
         <TextInput
           placeholder="Book Title"
           value={title}
           onChangeText={setTitle}
         />
         <TextInput
           placeholder="Author"
           value={author}
           onChangeText={setAuthor}
         />
         <Button title="Share" onPress={shareBook} />
       </View>
     );
   };

   export default ShareBookScreen;
   ```

### Step 5: Testing the App

1. **Run the App**:
   Make sure your Book Share API is running. Then, in the root of your React Native project, run:
   ```bash
   npx react-native run-android
   ```
   or
   ```bash
   npx react-native run-ios
   ```

### Step 6: Final Touches

- **Error Handling**: Enhance error handling for API requests.
- **Styling**: Use styles to make your app visually appealing.
- **User Authentication**: Implement user login to link shared books with specific users.

### Conclusion

Now you have a basic React Native app that can consume the Book Share API. You can expand it with features like user authentication, book details, and more. Enjoy building your app!

### Using Expo
Yes, you can definitely use Expo to build your React Native app! Expo simplifies the development process by providing a set of tools and services that streamline the setup and deployment of your React Native applications. Here’s how to create the **Book Share** app using Expo:

### Step 1: Set Up Your Expo Environment

1. **Install Expo CLI**:
   If you haven't already, install the Expo CLI:
   ```bash
   npm install -g expo-cli
   ```

2. **Create a New Expo Project**:
   ```bash
   expo init BookShareApp
   cd BookShareApp
   ```

3. **Choose a Template**: When prompted, choose a template (e.g., "blank" or "tabs") depending on your preference.

### Step 2: Install Required Dependencies

1. **React Navigation**:
   ```bash
   npm install @react-navigation/native @react-navigation/native-stack
   ```

2. **Install Required Dependencies for Navigation**:
   ```bash
   expo install react-native-gesture-handler react-native-reanimated react-native-screens react-native-safe-area-context @react-native-community/masked-view
   ```

3. **Axios** (for making API calls):
   ```bash
   npm install axios
   ```

### Step 3: Set Up Navigation

1. **Create a Navigation Container**:
   In your `App.js`, set up the navigation:
   ```javascript
   import * as React from 'react';
   import { NavigationContainer } from '@react-navigation/native';
   import { createNativeStackNavigator } from '@react-navigation/native-stack';
   import HomeScreen from './screens/HomeScreen';
   import ShareBookScreen from './screens/ShareBookScreen';

   const Stack = createNativeStackNavigator();

   export default function App() {
     return (
       <NavigationContainer>
         <Stack.Navigator initialRouteName="Home">
           <Stack.Screen name="Home" component={HomeScreen} />
           <Stack.Screen name="ShareBook" component={ShareBookScreen} />
         </Stack.Navigator>
       </NavigationContainer>
     );
   }
   ```

### Step 4: Create Screens

1. **Create a Home Screen**:
   In `screens/HomeScreen.js`, fetch and display the shared books:
   ```javascript
   import React, { useEffect, useState } from 'react';
   import { View, Text, Button, FlatList } from 'react-native';
   import axios from 'axios';

   const HomeScreen = ({ navigation }) => {
     const [books, setBooks] = useState([]);

     useEffect(() => {
       const fetchBooks = async () => {
         const response = await axios.get('http://your-api-url.com/books');
         setBooks(response.data);
       };
       fetchBooks();
     }, []);

     return (
       <View style={{ padding: 20 }}>
         <Button title="Share a Book" onPress={() => navigation.navigate('ShareBook')} />
         <FlatList
           data={books}
           keyExtractor={(item) => item.id.toString()}
           renderItem={({ item }) => (
             <View style={{ marginVertical: 10 }}>
               <Text>{item.title} by {item.author}</Text>
             </View>
           )}
         />
       </View>
     );
   };

   export default HomeScreen;
   ```

2. **Create a Share Book Screen**:
   In `screens/ShareBookScreen.js`, create a form to share a book:
   ```javascript
   import React, { useState } from 'react';
   import { View, TextInput, Button, Alert } from 'react-native';
   import axios from 'axios';

   const ShareBookScreen = ({ navigation }) => {
     const [title, setTitle] = useState('');
     const [author, setAuthor] = useState('');

     const shareBook = async () => {
       try {
         await axios.post('http://your-api-url.com/books', {
           title,
           author,
           user_id: 1 // Replace with actual user ID
         });
         Alert.alert('Success', 'Book shared successfully!');
         navigation.goBack();
       } catch (error) {
         Alert.alert('Error', error.message);
       }
     };

     return (
       <View style={{ padding: 20 }}>
         <TextInput
           placeholder="Book Title"
           value={title}
           onChangeText={setTitle}
           style={{ borderWidth: 1, marginBottom: 10, padding: 8 }}
         />
         <TextInput
           placeholder="Author"
           value={author}
           onChangeText={setAuthor}
           style={{ borderWidth: 1, marginBottom: 10, padding: 8 }}
         />
         <Button title="Share" onPress={shareBook} />
       </View>
     );
   };

   export default ShareBookScreen;
   ```

### Step 5: Running the App

1. **Run the App**:
   In the root of your Expo project, run:
   ```bash
   expo start
   ```
   This will open a new browser window with a QR code that you can scan using the Expo Go app on your mobile device, or you can run it in an emulator.

### Step 6: Final Touches

- **Styling**: Enhance the UI with additional styles to improve usability and aesthetics.
- **User Authentication**: Consider implementing user authentication for a personalized experience.
- **Testing**: Test the app thoroughly to ensure all features work correctly.

### Conclusion

Using Expo to build your **Book Share** app simplifies the development process and allows for easy deployment. You can further expand the app with features like user profiles, book reviews, or notifications. Enjoy building your app!
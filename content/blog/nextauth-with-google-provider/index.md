---
title: NextAuth With Google Provider
date: "2024-09-30T22:40:32.169Z"
description: Setting Up NextAuth With Google Provider.
---

You can include the `scope` parameter in the `params` object of the `authorization` configuration for the `GoogleProvider` in NextAuth. Configuring scope is essential if you want to request specific information from the user’s Google account, such as their email and profile information.

Here's how to structure it:

```javascript
import NextAuth from 'next-auth';
import GoogleProvider from 'next-auth/providers/google';

export default NextAuth({
  providers: [
    GoogleProvider({
      clientId: process.env.GOOGLE_CLIENT_ID as string,
      clientSecret: process.env.GOOGLE_CLIENT_SECRET as string,
      authorization: {
        params: {
          access_type: "offline",
          prompt: "consent",
          response_type: "code",
          scope: "openid email profile", 
        },
      },
    }),
  ],
  // Other NextAuth options...
});
```

### Breakdown of Parameters:
- **access_type**: Setting it to `offline` allows you to get a refresh token, enabling access when the user is not actively using your application.
- **prompt**: Using `consent` ensures that the user sees the consent screen every time.
- **response_type**: Typically set to `code` for authorization code flow.
- **scope**: Specifies what data you want to access. Common scopes include `openid`, `email`, and `profile`.

### Note:
Ensure that your Google Cloud Platform settings for the OAuth 2.0 consent screen include the scopes you're requesting. If you run into issues, double-check that your API keys and permissions are correctly configured.


### ID Token Versus Access Token
In Google authentication, the ID Token and Access Token serve different purposes:

### Use ID Token When:
- **Authentication**: The ID Token is primarily used to verify the identity of the user. It contains user information (claims) such as their email, name, and profile picture.
- **Client-side validation**: You can use the ID Token to confirm that the user has authenticated with Google.
- **Single Sign-On (SSO)**: It’s used to establish a user's session in your application after they've logged in with Google.

### Use Access Token When:
- **Authorization**: The Access Token is used to authorize API requests on behalf of the user. It allows your application to access specific resources or services provided by Google APIs (like Google Drive, Calendar, etc.).
- **Making API Calls**: When you need to interact with Google services, you'll send the Access Token in the authorization header of your requests.

### Summary:
- Use **ID Token** to authenticate users and verify their identity.
- Use **Access Token** to access Google APIs on behalf of the user and perform actions permitted by their scopes.

### Example Scenario:
- After a user logs in, you can use the ID Token to create a session in your app. Later, if you want to fetch the user's Google Drive files, you'd use the Access Token to authorize that API request.

### Further Reading
https://next-auth.js.org/providers/google


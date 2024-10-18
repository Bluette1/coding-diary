---
title: How To Deploy a Gatsby Site on GitHub Pages
date: "2024-10-17T22:40:32.169Z"
description: How To Deploy a Gatsby Site on GitHub Pages.
---

### Step 1: Build Your Gatsby Site with `--prefix-paths`

1. **Navigate to Your Project Directory**:
   Open your terminal and navigate to your Gatsby project folder.

   ```bash
   cd your-gatsby-site
   ```

2. **Build the Site with the `--prefix-paths` Flag**:
    When deploying a Gatsby site on GitHub Pages, especially if you're using a custom path prefix (which is often the case), you can use the `--prefix-paths` flag during the build process. This helps ensure that your assets are correctly referenced. This flag is useful if you have set a `pathPrefix` in your `gatsby-config.js`.

   ```bash
   gatsby build --prefix-paths
   ```

### Step 2: Configure `gatsby-config.js`

Ensure your `gatsby-config.js` file has the correct `pathPrefix` set:

```javascript
module.exports = {
  pathPrefix: "/your-repo-name", // replace with your actual repository name
  // Other configurations...
};
```

### Step 3: Install `gh-pages` (if not already installed)

If you haven't installed the `gh-pages` package yet, do so:

```bash
npm install gh-pages --save-dev
```

### Step 4: Add Deployment Script

Update your `package.json` to include a deployment script that also uses the `--prefix-paths` flag:

```json
"scripts": {
  "deploy": "gatsby build --prefix-paths && gh-pages -d public"
}
```

### Step 5: Deploy the Site

Run the deployment script:

```bash
npm run deploy
```

This command builds your site with the prefix paths and publishes the contents of the `public` folder to the `gh-pages` branch.

### Step 6: Configure GitHub Repository

1. **Go to Your Repository Settings**:
   Navigate to your GitHub repository and go to the **Settings** tab.

2. **GitHub Pages Section**:
   - In the **Pages** section, select the `gh-pages` branch as the source.
   - Save the changes.

3. **Custom Domain (Optional)**:
   If you want a custom domain, you can add it in the GitHub Pages settings.

### Step 7: Access Your Deployed Site

After a few minutes, your site should be available at:

```
https://<your-username>.github.io/<your-repo-name>/
```

### Additional Notes

- **Using `--prefix-paths`**: This flag is particularly useful to ensure that all internal links and asset paths are correctly prefixed with your repository name, preventing broken links.
- **Updating Your Site**: Each time you make changes, remember to run `npm run deploy` to update your site.
- **Debugging**: If you encounter issues, check the browser's console or the deployment logs for errors.
- **CORS Issues**: Be mindful of potential CORS issues if your site interacts with external APIs while hosted on GitHub Pages.

To set up a custom domain for your Gatsby site on GitHub Pages, you'll need to add a `CNAME` file to your repository. Here’s how to do it step-by-step:

### Step 1: Configure Your Custom Domain

1. **Purchase a Domain**: If you haven't already, purchase a domain from a domain registrar.
2. **Set Up DNS Records**: Go to your domain registrar’s DNS settings and create a CNAME record that points your domain (e.g., `www.yourdomain.com`) to `your-username.github.io`. If you want to use a root domain (e.g., `yourdomain.com`), you may need to set up an A record pointing to GitHub's IP addresses:
   - `185.199.108.153`
   - `185.199.109.153`
   - `185.199.110.153`
   - `185.199.111.153`

### Step 2: Add a CNAME File in Your Gatsby Project

1. **Create a CNAME File**: In your Gatsby project, create a file named `CNAME` (with no file extension) in the `static` directory. If the `static` directory does not exist, create it at the root of your project:

   ```
   your-gatsby-site/
   ├── gatsby-config.js
   ├── package.json
   ├── static/
   │   └── CNAME
   ```

2. **Add Your Domain**: Open the `CNAME` file and add your custom domain (e.g., `www.yourdomain.com` or `yourdomain.com`) as the only line in the file.

### Step 3: Update Your Deployment Script

Make sure your deployment script in `package.json` includes the CNAME file when you deploy:

```json
"scripts": {
  "deploy": "gatsby build --prefix-paths && gh-pages -d public -c CNAME"
}
```

### Step 4: Deploy Your Site

Run the deployment command:

```bash
npm run deploy
```

This will build your site, include the `CNAME` file in the `public` directory, and publish it to the `gh-pages` branch.

### Step 5: Configure GitHub Repository

1. **Go to Your Repository Settings**:
   Navigate to your GitHub repository and go to the **Settings** tab.

2. **GitHub Pages Section**:
   - In the **Pages** section, ensure that your custom domain is set under the "Custom domain" field.
   - Save the changes.

### Step 6: Wait for DNS Propagation

After setting everything up, it may take some time (anywhere from a few minutes to 48 hours) for DNS changes to propagate. Once completed, you should be able to access your site via your custom domain.

### Additional Notes

- **HTTPS Configuration**: GitHub Pages supports HTTPS for custom domains. Once your custom domain is set up, you can enable HTTPS in the GitHub Pages settings.
- **Testing**: You can test your setup by opening your custom domain in a browser. If everything is configured correctly, your Gatsby site should load.
- **Debugging**: If you encounter issues, double-check your DNS settings and ensure that the `CNAME` file is correctly placed and formatted.

Sure! Here’s a section you can add to your blog article to discuss the loading paths of static assets like fonts and images when deploying a Gatsby site on GitHub Pages:

### Handling Static Assets in Gatsby on GitHub Pages

When deploying your Gatsby site on GitHub Pages, it’s crucial to ensure that your static assets—such as images, fonts, and other media—load correctly. Here's how to manage the loading paths of these assets:

#### 1. **Use the `gatsby-plugin-image` for Images**

Gatsby provides a powerful image processing plugin that optimizes images for you. When using `gatsby-plugin-image`, you don’t need to worry about the paths since the plugin handles them correctly based on your `pathPrefix`.

Make sure you have the plugin installed and configured in your `gatsby-config.js`:

```bash
npm install gatsby-plugin-image gatsby-plugin-sharp gatsby-transformer-sharp
```

In `gatsby-config.js`:

```javascript
module.exports = {
  plugins: [
    `gatsby-plugin-image`,
    `gatsby-plugin-sharp`,
    `gatsby-transformer-sharp`,
    // Other plugins...
  ],
};
```

#### 2. **Referencing Static Assets**

When you reference static assets in your code, use relative paths. For example, if you have an image in the `static` folder, you can access it using:

```javascript
<img src="/your-image.png" alt="Description" />
```

This approach will ensure that the image path resolves correctly regardless of your deployment prefix.

#### 3. **Using the `Static Folder`**

Assets placed in the `static` folder of your Gatsby project will be served from the root path of your site. This means that if you have an image in `static/images`, you can reference it like so:

```javascript
<img src="/images/your-image.png" alt="Description" />
```

Make sure to use a leading slash to avoid path issues.

#### 4. **Fonts and Other Media**

For fonts, if you’re importing them in your CSS, ensure your CSS file correctly references the font paths. If your fonts are stored in a `static/fonts` directory, reference them as follows:

```css
@font-face {
  font-family: 'YourFont';
  src: url('/fonts/your-font.woff2') format('woff2'),
       url('/fonts/your-font.woff') format('woff');
}
```

Again, use a leading slash to point to the correct path.

#### 5. **Verifying Asset Loading**

After deploying your site, check the browser’s developer tools (F12) to see if any assets are failing to load. If you notice 404 errors for static assets, revisit your paths in the code and ensure they match the directory structure.


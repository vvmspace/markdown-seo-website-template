
---
title: "How to Add Netlify Deployment Status Badge to Hugo Template"
description: "Learn how to add a Netlify deployment status badge to your Hugo website template. This guide covers how to get the picture URL, configure parameters, and add the badge to the footer."
tags: ["Hugo", "Netlify", "Deployment", "Status Badge"]
---

Adding a Netlify deployment status badge to your Hugo template is a great way to showcase the status of your site. In this guide, we'll walk you through the steps to get the picture URL, configure the necessary parameters, and add the badge to your site's footer.

## Step 1: Get the Netlify Deployment Status Badge URL

Netlify provides a URL for the deployment status badge that you can use directly in your Hugo template. To find your badge URL:

1. Go to your Netlify dashboard.
2. Select the site you want to add the badge for.
3. Navigate to **Site Settings** > **Status Badges**.
4. Copy the Markdown image URL provided. It should look something like this:
   ```
   ![Netlify Status](https://api.netlify.com/api/v1/badges/YOUR-BADGE-ID/deploy-status)
   ```

## Step 2: Configure Parameters in `config.toml`

To make the badge URL configurable, we will add a new parameter in your Hugo site's configuration file. Open your `config.toml` file and add the following lines:

```toml
[params]
  netlifyBadgeURL = "https://api.netlify.com/api/v1/badges/YOUR-BADGE-ID/deploy-status"
```

Replace `YOUR-BADGE-ID` with the actual badge ID you copied from the Netlify dashboard.

## Step 3: Add the Badge to the Footer

Now that we have the badge URL configured, we can add it to the footer of our Hugo template. Open the footer template file, usually located at `layouts/partials/footer.html`. Add the following code to display the Netlify status badge:

```html
<footer>
  <div class="footer-content">
    <!-- Other footer content -->
    <div class="netlify-badge">
      <img src="{{ .Site.Params.netlifyBadgeURL }}" alt="Netlify Status">
    </div>
  </div>
</footer>
```

This code snippet will add the Netlify badge image to your site's footer, using the URL defined in your configuration file.

## Conclusion

By following these steps, you've successfully added a Netlify deployment status badge to your Hugo template's footer. This badge provides a visual indicator of your site's deployment status, which can be useful for both you and your visitors.

Feel free to adjust the styling of the badge by adding custom CSS to match your site's design. Happy coding!

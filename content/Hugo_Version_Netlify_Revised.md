---
title: How to Specify a Hugo Version for Netlify Deployment
description: Learn how to specify a Hugo version for your Netlify deployment to ensure consistency between your local environment and the production environment.
tags: [Hugo, Netlify, Deployment, Version Control]
---

# How to Specify a Hugo Version for Netlify Deployment

When deploying a Hugo site on Netlify, it’s crucial to ensure that the version of Hugo used locally matches the one used on Netlify. This consistency prevents discrepancies between your development environment and the production environment, which could lead to unexpected behavior or build failures. Here’s a step-by-step guide on how to specify a Hugo version for your Netlify deployment.

## Step 1: Determine Your Local Hugo Version

Before configuring Netlify, you need to know which version of Hugo you are currently using locally. This ensures that the same version is used during the Netlify build process. You can find out your Hugo version by running the following command in your terminal:

```bash
hugo version
```

This command will display the version of Hugo you have installed, such as `Hugo Static Site Generator v0.80.0/extended darwin/amd64 BuildDate: unknown`.

## Step 2: Configure Your `netlify.toml` File

The `netlify.toml` file is used to configure your Netlify build settings. If you don’t already have a `netlify.toml` file in your project’s root directory, you should create one. Here’s how you can specify the Hugo version:

1. **Open your text editor** and create a new file named `netlify.toml` in the root of your project.

2. **Add the following lines** to the `netlify.toml` file, replacing `0.80.0` with the version number you obtained from your local environment:

    ```toml
    [build]
      publish = "public"
      command = "hugo"

    [context.production.environment]
      HUGO_VERSION = "0.80.0"
    ```

    This configuration does the following:
    - It sets the directory to which your site should be published (`public`).
    - It specifies the build command (`hugo`).
    - It sets the Hugo version for the production environment.

3. **Save** the file.

## Step 3: Push Changes to Your Repository

After you’ve configured the `netlify.toml` file, you need to push the changes to your Git repository. Use the following Git commands:

```bash
git add netlify.toml
git commit -m "Specify Hugo version for Netlify build"
git push
```

## Step 4: Verify the Configuration on Netlify

Once you’ve pushed your changes, Netlify will trigger a new build. You can check the build logs on your Netlify dashboard to ensure that the specified Hugo version is being used. If there are any issues with the build, the logs will provide you with the necessary information to troubleshoot.

## Conclusion

Specifying the Hugo version in your Netlify deployment ensures that your site builds correctly and mirrors your local development environment, helping to avoid any build-time surprises. By following the steps outlined above, you can manage your Hugo version on Netlify with ease, leading to a more stable and predictable deployment process.

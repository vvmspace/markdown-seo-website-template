---
title: How to create a site with Hugo and deploy it to Netlify
tags: ["hugo", "netlify", "website", "deployment"]
description: A concise guide on creating a website with Hugo and deploying it to Netlify.
---

Creating a website with Hugo and deploying it to Netlify involves a series of steps. Here's a concise guide on how to set this up, assuming you already have a basic understanding of how to use your terminal and Git.

Check also:
- [How to Host Obsidian Notes on Netlify using Hugo with SEO template](/How_to_Host_Obsidian_Markdown_Website_Netlify_Hugo)
- [How to Specify a Hugo Version for Netlify Deployment](/Hugo_Version_Netlify_Revised)

## 1: Install Hugo

First, you need to install Hugo on your system. Since you're using macOS, you can install Hugo via Homebrew:

### Mac: 
```bash
brew install hugo
hugo version
```

### Windows: 
```bash
choco install hugo -confirm
hugo version
```

### Linux: 
```bash
snap install hugo --channel=extended
hugo version
```

## 2: Create a New Hugo Site

Create a new site using Hugo:

```bash
hugo new site how2gpt.xyz
cd how2gpt.xyz
```

### 3: Add a Theme

Add a theme from the Hugo themes directory. For example, to add the "Hugo SEO Theme" theme:
```bash
git init git submodule add https://github.com/vvmspace/hugo-seo-theme.git vvmspace/hugo-seo-theme
echo 'theme = "hugo-seo-theme"' >> config.toml
```

### 4: Add Some Content

```bash
hugo new posts/my-first-post.md
```

Edit `content/posts/my-first-post.md` in your text editor and add some content.

[How to start writing in Markdown](/how_to_start_writing_in_markdown)

### 5: Run Hugo Locally

Start the Hugo server with:

```bash
hugo server
```

Visit `http://localhost:1313` to see your new website.

### 6: Prepare for Deployment

Make sure your website's content is ready for deployment. Stop the server (Ctrl + C), and run:

```bash
hugo
```
This command builds your static site into the `public/` directory.

### 7: Deploy to Netlify

1. Set up `baseURL` in your `config.toml` file:

```toml
baseURL = "https://how2gpt.netlify.app/"
```

2. **Push your code to GitHub:**
    
    - Create a new repository on GitHub and name it `how2gpt.xyz`.
    - Initialize the local repository and push the code:
    
```bash
git add .
git commit -m "Initial commit"
git branch -M main
git remote add origin https://github.com/yourusername/how2gpt.xyz.git
git push -u origin main
```
    
3. **Set up Netlify:**
    
    - Go to [Netlify](https://netlify.com/) and sign in.
    - Click "New site from Git" and select GitHub.
    - Choose the repository you just pushed.
    - Set the build command to `hugo` and the publish directory to `public/`.
    - Click "Deploy site".

Netlify will automatically deploy your site and provide you with a URL to access it. You can also set up a custom domain in Netlify's domain management settings to use `how2gpt.xyz`.
# Static Website Deployment with CI/CD and GitHub Self-Hosted Runners

## Project Overview

This project demonstrates the deployment of a static website using CI/CD practices with GitHub Actions and a self-hosted runner. The website is deployed to an NGINX web server hosted on a cloud platform. The setup ensures automated deployment whenever changes are pushed to the `main` branch.

## Tech Stack

- **Cloud Platform:** AWS EC2 (can be substituted with Azure or GCP)
- **Web Server:** NGINX
- **CI/CD:** GitHub Actions with self-hosted runners
- **Languages:** HTML, CSS, JavaScript

## Features

- Automated deployment using GitHub Actions
- Self-hosted runner for CI/CD pipeline
- Exclusion of unnecessary files from deployment
- Static website featuring personal details and a link to HNG Internship

## Prerequisites

- AWS EC2 instance (or equivalent cloud instance)
- SSH access to the cloud instance
- GitHub repository with GitHub Actions enabled
- Self-hosted runner registered with GitHub

## Setup Instructions

### 1. Launch Cloud Instance

1. Launch an instance on your preferred cloud platform (AWS EC2, Azure, GCP).
2. Ensure the instance has a public IP and is accessible via SSH.

### 2. Register Self-Hosted Runner

1. Follow the [GitHub documentation](https://docs.github.com/en/actions/hosting-your-own-runners/adding-self-hosted-runners) to set up and register a self-hosted runner.
2. Make sure the runner has the necessary permissions to execute commands on your cloud instance.

### 3. Configure GitHub Actions Workflow

Create a `.github/workflows/deploy-website.yml` file in your repository with the following content:

```yaml
name: Deploy Static Website

on:
  push:
    branches:
      - main

jobs:
  deploy:
    runs-on: self-hosted

    steps:
      - name: Checkout repository
        uses: actions/checkout@v2

      - name: Install NGINX
        run: |
          sudo apt update
          sudo apt install -y nginx

      - name: Copy website files to NGINX directory
        run: |
          sudo rsync -av --exclude='.git*' $GITHUB_WORKSPACE/ /var/www/html/

      - name: Start and enable NGINX
        run: |
          sudo systemctl start nginx
          sudo systemctl enable nginx

      - name: Check NGINX status
        run: |
          sudo systemctl status nginx
```

### 4. Prepare Static Website

Ensure your static website files are ready for deployment. Place the following files in the root of your repository:

- index.html
- styles.css
- script.js

### 5. Deploy Website

Push your changes to the main branch.
GitHub Actions will automatically trigger the workflow, deploying your website to the NGINX server on the cloud instance.

### 6. Verify Deployment

Access your website using the public IP address of your cloud instance to verify the successful deployment.

## Static Website Content

HTML (index.html)
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>HNG Internship DevOps Track</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <header>
        <h1>Welcome to HNG Internship</h1>
        <p>DevOps Track - Static Website Deployment</p>
    </header>
    <main>
        <section>
            <h2>About Me</h2>
            <p>Name: [Your Name]</p>
            <p>Username: [Your Username]</p>
            <p>Email: [Your Email]</p>
        </section>
        <section>
            <h2>Learn More About HNG Internship</h2>
            <a href="https://hng.tech" target="_blank">HNG Internship</a>
        </section>
    </main>
    <footer>
        <p>&copy; 2024 HNG Internship DevOps Track</p>
    </footer>
    <script src="script.js"></script>
</body>
</html>
```

## CSS (styles.css)
```css
body {
    font-family: Arial, sans-serif;
    margin: 0;
    padding: 0;
    background-color: #f4f4f4;
    color: #333;
}

header {
    background-color: #4CAF50;
    color: white;
    padding: 20px 0;
    text-align: center;
}

main {
    padding: 20px;
}

section {
    margin-bottom: 20px;
}

a {
    color: #4CAF50;
    text-decoration: none;
}

a:hover {
    text-decoration: underline;
}

footer {
    background-color: #333;
    color: white;
    text-align: center;
    padding: 10px 0;
    position: fixed;
    width: 100%;
    bottom: 0;
}
```

## JavaScript (script.js)
```.js
document.addEventListener('DOMContentLoaded', function() {
    console.log('Website Loaded Successfully!');
});
```

### Conclusion

This project provides a foundation for deploying static websites using modern CI/CD practices. By integrating GitHub Actions and a self-hosted runner, the deployment process is streamlined, ensuring that your website is always up-to-date with the latest changes.

# Introduction
Hexo is a fast, simple and powerful blog framework. 
There are plenty of plugins and beautiful themes that could be used to customerize personal blog and most importantly, you could write posts using markdown. 

However, since hexo is actually a static website generator, there is no user interface to edit the posts like word press, you have to 'develop' your posts rather than to write your posts.  

Our target here is to integrate the netlify-cms to hexo, which enables the word press posts writting experience while keeping all benefits from hexo.

    


## Step 1: Download Code Template

```bash
git clone https://github.com/wuhaibo/hexo_blog_template.git
```

## Step 2: Create Repo in Gitlab and Upload the code From Step 1 to Gitlab Repo

Note: remove the .git folder in code folder from step 1.

## Step 3: Create a Site for Hosting in Netlify
- Sign in Netlify using Gitlab Credentials.
- Create a Site: New site from git -> choose the repo we created in Step 2. 
- Create User for netlify CMS 
  - Go to Site Settings -> Identity -> Enable Identity
    <img src="https://res.cloudinary.com/dr8wkuoot/image/upload/v1632410575/blog/EnableIdentity_elc1qq.jpg">
  - Choose Edit settings in Site ettings -> Identity -> Registration -> Registration preferences and select invite only.
  <img src="https://res.cloudinary.com/dr8wkuoot/image/upload/v1632410732/blog/InviteOnly_bsdilt.jpg">
  - Invite the user
  - Site ettings -> Identity -> Enable Git Gateway
- Preparation for Gitlab CI/CD
  - Get your Netlify Site API ID. Go to your site's setting page in Netlify Dashboard can copy the value of API ID
  - Get your Netlify Personal Access. Go to User Settings -> Applications -> Personal Access Token and generate a new access token. You could put "Gitlab CICD" as the description of your token. Once generated, copy it or save it in a file, if you leave the page, you would never be able to copy the token again. 
- Disable build in Netlify. Go to Site ettings -> Build & Deploy -> Build settings -> Stop builds
<img src="https://res.cloudinary.com/dr8wkuoot/image/upload/v1632412418/blog/stopBuilds_r5y9fz.jpg">
  



## Step4: Setup Gitlab CI/CD
- Open the repo on Gitlab and go to Settings -> CI/CD
- Add the API ID from last step under the variable name NETLIFY_SITE_ID 
- Add the access token got from last step under the variable name NETLIFY_AUTH_TOKEN
- add a new file gitlab-ci.yml to repo and replace with the actual url.

```yaml
image: node:latest

cache:
  paths:
    - node_modules/

stages:
  - deploy

deploy:
  stage: deploy
  environment:
    name: production
    url: https://your_site_url
  only:
    - master
  script:
    - npm i
    # your build command
    - npm run build
    - npx netlify-cli deploy --site $NETLIFY_SITE_ID --auth $NETLIFY_AUTH_TOKEN --prod
```
- push the changes to gitlab and validate.



## Step 5: Adding new Post
go to http://your_site_name.netlify.app/admin
login with the user created in step 3. Then you could add new posts. 

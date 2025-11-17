Portfolio Deployment Guide (Git, Jenkins, \& Webhooks)



This guide walks you through the entire process of deploying your index.html portfolio using Git, GitHub, and a Jenkins pipeline.



Part 1: (User b) Initialize Git \& Push to GitHub



First, you need to get your code onto GitHub.



Create a New GitHub Repository:



Go to GitHub and log in.



Click the + icon in the top-right corner and select "New repository".



Give it a name (e.g., personal-portfolio)



Click "Create repository".



Initialize Git Locally:



Save the index.html and Jenkinsfile I provided into a new folder on your computer (e.g., my-portfolio).



Open your terminal or command prompt and navigate into that folder:



cd path/to/my-portfolio





Run the following commands to turn this folder into a Git repository and commit your files:



\# Initialize an empty Git repository

## git init



\# Add all files to be tracked

## git add .



\# Create your first commit

### git commit -m "Initial commit: Add portfolio website and Jenkinsfile"





Push to GitHub:



On the GitHub repo page you just created, copy the "push an existing repository" commands. They will look like this:



\# Set the 'origin' (your GitHub repo) as the remote destination

## git remote add origin https://github.com/DPB45/personal-portfolio



\# Rename your local branch to 'main' (common practice)

## git branch -M main



\# Push your 'main' branch to the 'origin' remote

## git push -u origin main





Refresh your GitHub page. You should now see index.html and Jenkinsfile in your repository.



Part 2: (User c) Create the Jenkins Pipeline



Now, let's teach Jenkins how to build this project.



Prerequisites:



A running Jenkins server.



The GitHub and Pipeline plugins installed (they are usually included by default).



Create a New Jenkins Job:



On your Jenkins dashboard, click "New Item" on the left.



Enter a name (e.g., portfolio-pipeline).



Select "Pipeline" as the job type.



Click "OK".



Configure the Pipeline:



On the configuration page, scroll down to the "Pipeline" section.



Change the "Definition" dropdown to "Pipeline script from SCM".



SCM: Select "Git".



Repository URL: Enter the .git URL of your GitHub repository (e.g., https://github.com/YOUR\_USERNAME/personal-portfolio.git).



Branch Specifier: Make sure it's \*/main.



Script Path: Type Jenkinsfile (this tells Jenkins to look for the file with that exact name in your repo).



Click "Save".



Run Manually (First Time):



Click "Build Now" on the left side of your new pipeline's page.



You'll see a build start in the "Build History". Click it to watch the "Console Output". It should go through the "Checkout" and "Build \& Deploy" stages successfully.



If it's successful, you'll see "Pipeline finished successfully" in the logs, and an "Artifacts" link will appear on the build page, where you can find your index.html.







## download ngrok

## 

## ngrok config add-authtoken 35VrWpvQLFppjYF6gEu5I87s7bz_31fxiKq6k1jq6FTsGyJVw

## 

## ngrok http 8080 



Part 3: (User d) Configure the GitHub Webhook



This is the final step: making GitHub automatically tell Jenkins to build every time you push a change.



Get Your ngrok URL:



Your Jenkins server's URL. It must be accessible from the public internet for GitHub to reach it.



## The full webhook URL will be: https://<YOUR\_ngrok\_IP\_OR\_DOMAIN>/github-webhook/



IMPORTANT: The trailing slash (/) is required.



Add the Webhook in GitHub:



Go to your repository on GitHub.



Click the "Settings" tab.



Click "Webhooks" from the left menu.



Click the "Add webhook" button.



Payload URL: Paste your full Jenkins webhook URL (e.g., http://123.45.67.89:8080/github-webhook/).



Content type: Change this to application/json.




Click "Add webhook".



you need to tell your Jenkins job to listen for the webhook.



Go to your Jenkins dashboard (e.g., http://localhost:8080).



Click on your job, portfolio-pipeline. (jenkins)



Click "Configure" on the left-hand menu.



Scroll down to the "Triggers" section.



Check the box that says "GitHub hook trigger for GITScm polling".



Click "Save" at the bottom.





Test the Full Pipeline:



On your local computer, open index.html.



Make a small change. For example, change <title>Your Name</title> to <title>Your Name - Deployed!</title>.



Save the file.



Go to your terminal and run:



\# Add your changes

## git add index.html



\# Commit the changes

## git commit -m "Test: Updated portfolio title"



\# Push to GitHub

## git push








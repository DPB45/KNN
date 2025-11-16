Portfolio Deployment Guide (Git, Jenkins, \& Webhooks)



This guide walks you through the entire process of deploying your index.html portfolio using Git, GitHub, and a Jenkins pipeline.



Part 1: (User b) Initialize Git \& Push to GitHub



First, you need to get your code onto GitHub.



Create a New GitHub Repository:



Go to GitHub and log in.



Click the + icon in the top-right corner and select "New repository".



Give it a name (e.g., personal-portfolio).



Make it Public (or Private, but Public is simpler for Jenkins access).



Do not initialize it with a README, .gitignore, or license.



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

## ngrok config add-authtoken (use your authtoken)

## 

## ngrok http 8080 



Part 3: (User d) Configure the GitHub Webhook



This is the final step: making GitHub automatically tell Jenkins to build every time you push a change.



Get Your Jenkins URL:



Your Jenkins server's URL. It must be accessible from the public internet for GitHub to reach it.



## The full webhook URL will be: https://<YOUR\_JENKINS\_IP\_OR\_DOMAIN>/github-webhook/



IMPORTANT: The trailing slash (/) is required.



Add the Webhook in GitHub:



Go to your repository on GitHub.



Click the "Settings" tab.



Click "Webhooks" from the left menu.



Click the "Add webhook" button.



Payload URL: Paste your full Jenkins webhook URL (e.g., http://123.45.67.89:8080/github-webhook/).



Content type: Change this to application/json.



Which events would you like to trigger this webhook? Select "Just the push event." This is the default and what you want.



Click "Add webhook".



you need to tell your Jenkins job to listen for the webhook.



Go to your Jenkins dashboard (e.g., http://localhost:8080).



Click on your job, portfolio-pipeline.



Click "Configure" on the left-hand menu.



Scroll down to the "Build Triggers" section.



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







Immediately go to your Jenkins dashboard. Within seconds, you should see your portfolio-pipeline job automatically start a new build!



You now have a fully automated CI/CD pipeline. Any change you git push to your main branch will automatically be "deployed" by Jenkins.







index.html



<!DOCTYPE html>

<html lang="en" class="scroll-smooth">

<head>

    <meta charset="UTF-8">

    <meta name="viewport" content="width=device-width, initial-scale=1.0">

    <title>Your Name - Personal Portfolio</title>

    <!-- Load Tailwind CSS from CDN -->

    <script src="https://cdn.tailwindcss.com"></script>

    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;700\\\&display=swap" rel="stylesheet">

    <style>

        body {

            font-family: 'Inter', sans-serif;

        }

        /\* Simple page transition \*/

        .page-section {

            display: none;

            animation: fadeIn 0.5s ease-in-out;

        }

        .page-section.active {

            display: block;

        }

        @keyframes fadeIn {

            from { opacity: 0; transform: translateY(10px); }

            to { opacity: 1; transform: translateY(0); }

        }

    </style>

</head>

<body class="bg-gray-900 text-white selection:bg-indigo-500 selection:text-white">



    <!-- Header \\\& Navigation -->

    <header class="sticky top-0 z-50 bg-gray-900/80 backdrop-blur-md border-b border-gray-700">

        <nav class="container mx-auto px-6 py-4 flex justify-between items-center">

            <a href="#" class="text-2xl font-bold nav-link" data-page="home">

                Your Name

            </a>

            <div class="space-x-6">

                <a href="#home" class="text-lg text-gray-300 hover:text-indigo-400 transition duration-300 nav-link" data-page="home">Home</a>

                <a href="#projects" class="text-lg text-gray-300 hover:text-indigo-400 transition duration-300 nav-link" data-page="projects">Projects</a>

            </div>

        </nav>

    </header>



    <!-- Main Content Area -->

    <div class="container mx-auto px-6 py-12">



        <!-- Page 1: Home -->

        <section id="home" class="page-section active min-h-\\\[70vh] flex items-center">

            <div class="max-w-3xl">

                <span class="text-indigo-400 font-semibold">Hello, I'm</span>

                <h1 class="text-5xl md:text-7xl font-bold mt-2 mb-4">Your Name</h1>

                <h2 class="text-3xl md:text-4xl text-gray-300 mb-6">A Passionate Web Developer</h2>

                <p class="text-lg text-gray-400 mb-8 leading-relaxed">

                    I specialize in building modern, responsive, and user-friendly web applications. From slick front-end interfaces to robust back-end logic, I love turning ideas into reality. This website itself is deployed via a CI/CD pipeline!

                </p>

                <a href="#projects" class="inline-block bg-indigo-600 text-white font-medium px-8 py-3 rounded-lg shadow-lg hover:bg-indigo-700 transition duration-300 transform hover:-translate-y-1 nav-link" data-page="projects">

                    View My Work

                </a>

            </div>

        </section>



        <!-- Page 2: Projects -->

        <section id="projects" class="page-section min-h-\\\[70vh]">

            <h2 class="text-4xl font-bold mb-8 text-center">My Projects</h2>

            <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-8">

 

                <!-- Project Card 1 -->

                <div class="bg-gray-800 rounded-lg shadow-xl overflow-hidden transform transition duration-500 hover:scale-105">

                    <img src="https://placehold.co/600x400/1f2937/7f9cf5?text=Project+Thumbnail" alt="Project 1" class="w-full h-48 object-cover">

                    <div class="p-6">

                        <h3 class="text-2xl font-bold mb-2">Project One</h3>

                        <p class="text-gray-400 mb-4">

                            A brief description of what this project does, the technologies used, and its purpose.

                        </p>

                        <a href="#" class="text-indigo-400 font-medium hover:underline">View Code \&rarr;</a>

                    </div>

                </div>



                <!-- Project Card 2 -->

                <div class="bg-gray-800 rounded-lg shadow-xl overflow-hidden transform transition duration-500 hover:scale-105">

                    <img src="https://placehold.co/600x400/1f2937/7f9cf5?text=Project+Thumbnail" alt="Project 2" class="w-full h-48 object-cover">

                    <div class="p-6">

                        <h3 class="text-2xl font-bold mb-2">Project Two</h3>

                        <p class="text-gray-400 mb-4">

                            A brief description of what this project does, the technologies used, and its purpose.

                        </p>

                        <a href="#" class="text-indigo-400 font-medium hover:underline">View Code \&rarr;</a>

                    </div>

                </div>



                <!-- Project Card 3 -->

                <div class="bg-gray-800 rounded-lg shadow-xl overflow-hidden transform transition duration-500 hover:scale-105">

                    <img src="https://placehold.co/600x400/1f2937/7f9cf5?text=Project+Thumbnail" alt="Project 3" class="w-full h-48 object-cover">

                    <div class="p-6">

                        <h3 class="text-2xl font-bold mb-2">Project Three</h3>

                        <p class="text-gray-400 mb-4">

                            A brief description of what this project does, the technologies used, and its purpose.

                        </p>

                        <a href="#" class="text-indigo-400 font-medium hover:underline">View Code \&rarr;</a>

                    </div>

                </div>



            </div>

        </section>



    </div>



    <!-- Footer -->

    <footer class="border-t border-gray-700 mt-16">

        <div class="container mx-auto px-6 py-8 text-center text-gray-500">

            <p>\&copy; 2025 Your Name. All rights reserved.</p>

        </div>

    </footer>



    <script>

        document.addEventListener('DOMContentLoaded', () => {

            const navLinks = document.querySelectorAll('.nav-link');

            const pageSections = document.querySelectorAll('.page-section');



            // Function to show the correct page

            const showPage = (pageId) => {

                // Hide all sections

                pageSections.forEach(section => {

                    section.classList.remove('active');

                });

 

                // Show the target section

                const targetPage = document.getElementById(pageId);

                if (targetPage) {

                    targetPage.classList.add('active');

                }



                // Update URL hash

                window.location.hash = '#' + pageId;

            };



            // Handle nav link clicks

            navLinks.forEach(link => {

                link.addEventListener('click', (e) => {

                    e.preventDefault();

                    const pageId = link.getAttribute('data-page');

                    showPage(pageId);

                });

            });



            // Show page based on URL hash on load

            const currentHash = window.location.hash.replace('#', '');

            if (currentHash === 'projects') {

                showPage('projects');

            } else {

                showPage('home'); // Default to home

            }

        });

    </script>

</body>

</html>







Jenkins file ( save as "Jenkinsfile" select all files )





/\* This is a Declarative Jenkins Pipeline file.

  It defines the entire CI/CD process.

\*/



pipeline {

    // 1. Agent: Specify where to run the pipeline. 'any' means any available agent.

    agent any



    // 2. Stages: Define the distinct parts of the pipeline.

    stages {

 

        // Stage 1: Checkout Code

        stage('Checkout') {

            steps {

                // This step checks out the code from your GitHub repository.

                // Jenkins will automatically get the URL from the job configuration.

                echo 'Checking out code...'

                checkout scm

            }

        }



        // Stage 2: "Build / Deploy"

        stage('Build \& Deploy') {

            steps {

                // For a static HTML/CSS site, there's no "build" step (like compiling).

                // Our "deploy" step will be to archive the website files.

                // In a real-world scenario, this step might use 'ssh' or 'scp'

                // to copy files to a live web server (e.g., /var/www/html).

                echo 'Building and deploying...'

 

                // This step packages the website files and saves them with the build.

                // You can download them from the Jenkins build page.

                archiveArtifacts artifacts: '\*.html', followSymlinks: false

            }

        }

    }



    // 3. Post-build Actions: Run steps after the pipeline finishes.

    post {

        success {

            // This will run only if the pipeline is successful.

            echo 'Pipeline finished successfully. Portfolio "deployed"!'

        }

        failure {

            // This will run only if the pipeline fails.

            echo 'Pipeline failed. Please review the logs.'

        }

    }

}


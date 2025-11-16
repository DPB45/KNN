Create a new repository on GitHub. Go to GitHub.com, click the "+" icon, and select "New repository." Give it a name (like blog-project) and click "Create repository."



Initialize Git in your folder. Open a terminal or command prompt, cd into your blog-project directory, and run:



Bash



## git init

## git add .

## git commit -m "Initial commit: Add blog project files"

## git branch -M main

## git remote add origin https://github.com/YOUR\_USERNAME/blog-project.git

## git push -u origin main



Step 2: Download and Install Maven

Go to the official Apache Maven download page: https://maven.apache.org/download.cgi



Look for the "Binary zip archive" (e.g., apache-maven-3.9.8-bin.zip).



Download the ZIP file.



Unzip this file to a permanent location on your computer where you keep your tools. A perfect spot is something like C:\\Program Files\\.



When you're done, you should have a folder path like C:\\Program Files\\apache-maven-3.9.8. This is your Maven home directory.



Step 3: Set Your Environment Variables (The Most Important Part)

This is how you tell Windows where to find Maven.



In the Windows Start Menu, type "env" or "environment".



Select "Edit the system environment variables".



A "System Properties" window will pop up. Click the "Environment Variables..." button at the bottom.



Now you'll see two sections: "User variables" and "System variables." We will work in the "System variables" section.



Create MAVEN\_HOME:



Click the "New..." button under "System variables."



Variable name: MAVEN\_HOME



Variable value: C:\\Program Files\\apache-maven-3.9.8 (or the full path to wherever you unzipped Maven)



Click OK.



Add Maven to your Path:



In the "System variables" list, find and select the variable named "Path".



Click the "Edit..." button.



A new window will open showing a list of paths. Click the "New" button on the right.



Type in the following text exactly: %MAVEN\_HOME%\\bin



Click "OK" on all the windows you opened (the "Edit" window, the "Environment Variables" window, and the "System Properties" window) to save your changes.



## tocheck installation : mvn -version



## mvn clean package



## mvn jetty:run 


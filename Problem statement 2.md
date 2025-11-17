



Initialize Git: Start tracking your project files with Git.



Bash



# git init

Commit Files: Add the files and create the initial commit.



Bash



# git add .

# git commit -m "feat: Add initial HTML website files and Dockerfile"

# Push to GitHub (for repository creation):



Create a new, empty repository on GitHub (e.g., voyage-agency).



Follow the instructions provided by GitHub to link your local repository and push the files:



Bash



# git remote add origin <YOUR\_GITHUB\_REPO\_URL>

# git branch -M main

# git push -u origin main

(The project repository is now pushed to GitHub, fulfilling requirement 'b'.)



Step 2: Build the Docker Image (Part c)

This is where the Dockerfile is read and executed to create a self-contained image ready for deployment. This process copies your static files into the Nginx server environment.



Execute the Docker Build Command: Run the docker build command from the root of your travel-agency-docker directory.



Bash

# docker rm voyage-website (to remove previous container)

# docker build -t voyage-agency:latest .

-t voyage-agency:latest: Tags the resulting image with the name voyage-agency and the tag latest.



.: Tells Docker to look for the Dockerfile in the current directory.



Verify the Image Build: Check that the image was successfully created by listing your local Docker images.



Bash



 # docker images

You should see voyage-agency listed in the output.



Step 3: Run the Docker Container and Verify (Part d)

This final step launches the server using the image and maps the internal web port to your host machine's port.


Bash


# docker run -d -p 8085:80 --name voyage-website voyage-agency:latest

-d: Runs the container in detached (background) mode.



-p 8080:80: Publishes port 8085 (host) to port 80 (container).



--name voyage-website: Assigns an easily identifiable name to the running container.



Verify Container Status: Confirm that the container is running successfully.



Bash



# docker ps

You should see the voyage-website container with a status of "Up" and the correct port mapping (0.0.0.0:8080->80/tcp).



Verify Website Accessibility: Open your web browser and enter the following address:



http://localhost:8085

You should see the "Voyage Agency" homepage displayed, confirming the deployment was successful. You can click on "Destinations" to verify the second page is also served correctly.





**Simple Nodejs Docker App – Build, Test and deploy the code from Local to DockerHub**

**1. Get the Node.js code from the developer**

•	Clone or download the code from GitHub, GitLab, or zip.

    git clone <repo-url>
    cd <node-docker-app>

**2. Install Node.js on your base machine (laptop or server)**

  •	Download from https://nodejs.org 
  
  •	Or install via terminal:
  
    # For Ubuntu/Debian
    
    curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
    
    sudo apt install -y nodejs
    
    _**Verify installation:**___
      # node -v
      # npm -v

**3. Install project dependencies with npm**

Inside the project folder (where package.json is located):

    # npm install

    **#IMPORTANT#- Run the application locally to verify it works**

    # npm start

    Now, access it at:_http://localhost:PORT_

**Note:** update your code to listen on 3000 or any user port to avoid permission issues.

**4. Once verified locally, containerize with Docker**

Create a Dockerfile in the root of your project:

    FROM node:18
    WORKDIR /usr/src/app
    COPY package*.json ./
    COPY app.js ./
    RUN npm install
    EXPOSE 3000
    CMD ["node", "app.js"]

**5. Build the Docker image**

From the root of the project folder:

    #docker build -t node-docker-app:v1.0 .

**Run the container**

    #docker run -d -p 3000:3000 --name node-app-container node-docker-app:v1.0

**6. Test the app in your browser**

  Use your EC2 IP or localhost:
  
      #http://<EC2-IP>:3000
  
  Make sure port 3000 is allowed in your EC2 security group if you're testing from a remote location.

**7. Push Docker Image to Docker Hub or AWS ECR**

  Make sure you’re logged in DockerHub:
  
    #docker login
  
    #docker tag node-docker-app:v1.0 <dockerHub_username>/node-docker-app:v1.0
  
    #docker push <dockerHub_username>/node-docker-app:v1.0

Make sure you’re logged in AWS ECR:
    
    #aws ecr create-repository --repository-name react-docker-app

    #aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin <aws_account_id>.dkr.ecr.us-east-1.amazonaws.com

    #docker tag react-docker-app:latest <aws_account_id>.dkr.ecr.us-east-1.amazonaws.com/react-docker-app:latest

    #docker push <aws_account_id>.dkr.ecr.us-east-1.amazonaws.com/react-docker-app:latest


**8. Pull and Run From DockerHub or AWS ECR**

  DockerHub:
  
    #docker pull <dockerHub_username>/node-docker-app:v1.0
   
    #docker run -d -p 3000:3000 --name node-from-hub <dockerHub_username>/node-docker-app:v1.0

AWS ECR:

    #docker pull <aws_account_id>.dkr.ecr.us-east-1.amazonaws.com/node-docker-app:v1.0

    #docker run -d -p 3000:3000 --name node-ecr <aws_account_id>.dkr.ecr.us-east-1.amazonaws.com/node-docker-app:v1.0

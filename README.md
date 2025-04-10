Simple Nodejs Docker App – Local to Docker Deployment Steps by Step

1. Get the Node.js code from the developer
•	Clone or download the code from GitHub, GitLab, or zip.
git clone <repo-url>
cd <project-folder>

2. Install Node.js on your base machine (laptop or server)
•	Download from https://nodejs.org
•	Or install via terminal:
# For Ubuntu/Debian
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
sudo apt install -y nodejs
Verify installation:
node -v
npm -v

3. Install project dependencies with npm
Inside the project folder (where package.json is located):
npm install

4. Run the application locally to verify it works
node server.js
Now, access it at:
http://localhost:PORT
⚠️ If server.js is listening on port 80, you might need to use sudo node server.js
Better: update your code to listen on 3000 or any user port to avoid permission issues.

5. Once verified locally, containerize with Docker
Create a Dockerfile in the root of your project:
FROM node:18

WORKDIR /app

COPY package.json /app

RUN npm install

COPY . /app

EXPOSE 3000

CMD ["node", "server.js"]
Make sure server.js listens on:
const PORT = process.env.PORT || 3000;
app.listen(PORT, () => {
  console.log(`Server is running on port ${PORT}`);
});

6. Build the Docker image
From the root of the project folder:
docker build -t my-node-app .

7. Run the container
sudo docker run -p 3000:3000 my-node-app
To run it in the background:
sudo docker run -d -p 3000:3000 my-node-app

8. Test the app in your browser
Use your EC2 IP or localhost:
http://<EC2-IP>:3000
Make sure port 3000 is allowed in your EC2 security group if you're testing from a remote location.


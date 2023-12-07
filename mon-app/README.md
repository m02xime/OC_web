# Continuous Deployment with GitHub Actions

This repository is set up for continuous deployment using GitHub Actions. The workflow is triggered on each push to the `main` branch.

## Workflow Description

The workflow consists of the following steps:

1. **Checkout Repository:** Checks out the latest code from the repository.
2. **Setup Node.js:** Sets up Node.js version 14 for the build.
3. **Install Dependencies:** Changes to the 'mon-app' directory and installs the project dependencies using npm.
4. **Build:** Builds the application using the npm run build command.
5. **Setup SSH Key:** Uses the webfactory/ssh-agent action to configure the SSH key for secure deployment.
6. **Deploy:** Deploys the built application to the remote server using SCP.

### Secrets

Make sure to add the following secrets to your GitHub repository:

- `SSH_PRIVATE_KEY`: Your private SSH key for secure deployment.
- `REMOTE_USER`: The username for accessing the remote server.
- `REMOTE_HOST`: The IP address or hostname of the remote server.
- `REMOTE_TARGET`: The target directory on the remote server where the built application will be deployed.

## Dockerfile

The repository includes a Dockerfile for containerization of the application. It is configured to use the official Node.js 14 image as the base image, install dependencies, build the application, and specify the command to run on container startup.

```dockerfile
# Use an official Node.js runtime as a parent image
FROM node:14

# Set the working directory to /app
WORKDIR /app

# Copy package.json and package-lock.json to the working directory
COPY package*.json ./

# Install app dependencies
RUN npm install

# Copy the current directory contents into the container at /app
COPY . /app

# Build the app
RUN npm run build

# Specify the command to run on container startup
CMD ["npm", "start"]

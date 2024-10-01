# Project Name - Thetha

This is the frontend part of the Project Name web application, built with React. It connects to a backend API and provides a responsive user interface for interacting with the application's features.

## Table of Contents

- [Getting Started](#getting-started)
  - [Prerequisites](#prerequisites)
  - [Installation](#installation)
  - [Running the Application](#running-the-application)
- [Project Structure](#project-structure)
- [Available Scripts](#available-scripts)
- [Environment Variables](#environment-variables)
- [Technologies](#technologies)
- [Linting](#linting)
- [Testing](#testing)
- [Deployment](#deployment)
  - [GitHub Actions](#github-actions)
  - [Docker Hub](#docker-hub)
  - [Terraform & EKS](#terraform--eks)
- [Contributing](#contributing)
- [License](#license)

## Getting Started

To get a local copy of the project up and running, follow these steps.

### Prerequisites

Ensure you have the following installed:

- Node.js (v14 or later)
- npm (or Yarn)

### Installation

1. Clone the repository:


git clone https://github.com/victormpala/thetha.git
cd your-project/frontend


2. Install dependencies:

```
npm install
```

OR (if using Yarn):

```
yarn install
```

### Running the Application

Start the development server:

```
npm start
```

OR (if using Yarn):

```
yarn start
```

The application should now be running at http://localhost:3000.

## Project Structure

```
frontend/
├── public/           # Static assets like HTML, images, etc.
├── src/
│   ├── assets/       # Project images, fonts, etc.
│   ├── components/   # Reusable UI components
│   ├── pages/        # Page-level components
│   ├── services/     # API calls and external services
│   ├── hooks/        # Custom React hooks
│   ├── styles/       # Global and shared styles
│   ├── App.js        # Main application component
│   └── index.js      # React app entry point
├── package.json      # Project metadata and dependencies
└── README.md         # Documentation
```

## Available Scripts

In the project directory, you can run:

- `npm start` or `yarn start`: Starts the development server.
- `npm run build` or `yarn build`: Builds the app for production.
- `npm test` or `yarn test`: Launches the test runner in the interactive watch mode.
- `npm run lint` or `yarn lint`: Runs linter checks on the project.
- `npm run eject` or `yarn eject`: Removes the single build dependency from your project.

## Environment Variables

To configure the application, you need to set up environment variables in a .env file at the root of the frontend directory. The most common environment variables are:


REACT_APP_API_URL=https://your-backend-api.com
REACT_APP_API_KEY=your-api-key-here


## Technologies

This project uses the following technologies:

- React: For building the user interface.
- React Router: For navigation and routing.
- Axios: For making HTTP requests.
- Styled-components (or any other styling library you are using).
- ESLint: For code linting.
- Jest/React Testing Library: For testing.

## Linting

To maintain code quality, ESLint is used for linting. You can run the linter to check for code formatting and style issues.

Run linting:

```
npm run lint
```

OR (if using Yarn):

```
yarn lint
```

Automatically fix linting issues:

```
npm run lint:fix
```

OR (if using Yarn):

```
yarn lint:fix
```

Make sure to run the linter before submitting any code for review to ensure consistent code quality.

## Testing

Testing is done using Jest and React Testing Library. Unit tests ensure that components behave as expected, while integration tests verify that the application works as a whole.

Run tests:

```
npm test
```

OR (if using Yarn):

```
yarn test
```

Run tests in watch mode:

```
npm test -- --watch
```

OR (if using Yarn):

```
yarn test --watch
```

Run coverage report:

```
npm run test:coverage
```

OR (if using Yarn):

```
yarn test:coverage
```

## Deployment

### GitHub Actions

GitHub Actions is used to automate the build and deployment of Docker images. The workflow is configured to:

1. Build the Docker image.
2. Push the Docker image to Docker Hub.



```
name: Build and Push Docker Image

on:
  push:
    branches:
      - main

jobs:
  build-and-push:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v2
      
      - name: Log in to Docker Hub
        run: echo "${{ secrets.DOCKER_PASSWORD }}" | docker login -u "${{ secrets.DOCKER_USERNAME }}" --password-stdin
      
      - name: Build Docker Image
        run: docker build -t your-dockerhub-username/your-image-name:latest .
      
      - name: Push Docker Image to Docker Hub
        run: docker push your-dockerhub-username/your-image-name:latest
```

### Docker Hub

After the Docker image is built, it is pushed to Docker Hub for storage and later use during the deployment phase.

### Terraform & EKS

Terraform is used to automate the infrastructure deployment. It provisions an Amazon EKS (Elastic Kubernetes Service) cluster. Once the cluster is up, Kubernetes YAML files are used to:

- Deploy the Docker images from Docker Hub to the Kubernetes cluster.
- Set up ingress and manage networking for the application.



```
provider "aws" {
  region = "your-aws-region"
}

module "eks" {
  source          = "terraform-aws-modules/eks/aws"
  cluster_name    = "your-cluster-name"
  cluster_version = "1.21"
  vpc_id          = "your-vpc-id"
  subnets         = ["subnet-1", "subnet-2"]

  # Other necessary configurations
}


Kubernetes Deployment Example of a Kubernetes YAML deployment file:


apiVersion: apps/v1
kind: Deployment
metadata:
  name: frontend-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: frontend
  template:
    metadata:
      labels:
        app: frontend
    spec:
      containers:
      - name: frontend
        image: your-dockerhub-username/your-image-name:latest
        ports:
        - containerPort: 80

```

The ingress configuration will manage how traffic reaches the deployed app.

## Contributing

We welcome contributions to this project! To contribute:

1. Fork the repository.
2. Create a new branch (`git checkout -b feature-branch`).
3. Make your changes.
4. Commit your changes (`git commit -m 'Add some feature'`).
5. Push to the branch (`git push origin feature-branch`).
6. Open a pull request.

## License

This project is licensed under the MIT License - see the LICENSE file for details.

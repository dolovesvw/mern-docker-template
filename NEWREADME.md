# MERN Stack Template with Docker - DevOps Workflow

## Overview

This is a template for a MERN stack (MongoDB, Express, React, Node.js) application, containerized with Docker, and includes additional technologies such as Tailwind CSS, Bootstrap, Material-UI, Webpack, TypeScript, ESLint, Vitest/Jest, React Router DOM, and Auth0. The repository follows DevOps best practices for continuous integration and delivery (CI/CD).

## DevOps Workflow

### 1. Plan

Start by planning your application requirements, including the necessary services and technologies you will integrate:

- **Frontend**: React with TypeScript, styled with Tailwind CSS, Bootstrap, and Material-UI.
- **Backend**: Node.js + Express with TypeScript, connected to MongoDB.
- **Containerization**: Docker for both client and server environments.
- **Authentication**: Auth0 for secure user authentication.
- **CI/CD Tools**: GitHub Actions (or other CI/CD tools) to automate the build, test, and deploy phases.

---

### 2. Code

Set up the project structure and initialize your codebase.

1. **Clone the Repository**: Start by cloning the repository from GitHub.

    ```bash
    git clone https://github.com/yourusername/mern-docker-template.git
    cd mern-docker-template
    ```

2. **Project Structure**: Create folders for the client and server.

    ```bash
    mkdir client server
    cd client
    npx create-react-app . --template typescript
    cd ../server
    npm init -y
    ```

---

### 3. Build

Install the necessary dependencies and build the project.

#### Client (React + TypeScript)

1. Install dependencies:

    ```bash
    cd client
    npm install axios react-router-dom @auth0/auth0-react @mui/material @emotion/react @emotion/styled tailwindcss bootstrap
    npm install --save-dev eslint jest @testing-library/react @testing-library/jest-dom
    ```

2. Set up **Tailwind CSS**:

    ```bash
    npx tailwindcss init
    ```

3. Update `tailwind.config.js`:

    ```javascript
    module.exports = {
      content: ['./src/**/*.{js,jsx,ts,tsx}', './public/index.html'],
      theme: {
        extend: {},
      },
      plugins: [],
    };
    ```

4. Update `src/index.css` to include Tailwind:

    ```css
    @tailwind base;
    @tailwind components;
    @tailwind utilities;
    ```

5. Set up **ESLint**:

    ```bash
    npx eslint --init
    ```

#### Server (Node.js + Express)

1. Install dependencies:

    ```bash
    cd ../server
    npm install express mongoose cors dotenv
    npm install --save-dev typescript @types/node @types/express ts-node
    ```

2. Create basic server setup in `src/index.ts`:

    ```typescript
    import express from 'express';
    import mongoose from 'mongoose';
    import cors from 'cors';
    import dotenv from 'dotenv';

    dotenv.config();

    const app = express();
    const PORT = process.env.PORT || 5000;

    app.use(cors());
    app.use(express.json());

    app.get('/', (req, res) => {
      res.send('Hello from the server!');
    });

    mongoose.connect(process.env.MONGODB_URI || 'mongodb://localhost:27017/mydb', { useNewUrlParser: true, useUnifiedTopology: true })
      .then(() => {
        app.listen(PORT, () => {
          console.log(`Server is running on port ${PORT}`);
        });
      })
      .catch(err => console.error(err));
    ```

3. Create a **tsconfig.json** for TypeScript:

    ```bash
    npx tsc --init
    ```

4. Set up Dockerfile in the `server` directory:

    ```Dockerfile
    FROM node:14
    WORKDIR /app
    COPY package*.json ./
    RUN npm install
    COPY . .
    CMD ["ts-node", "src/index.ts"]
    ```

---

### 4. Test

Add tests to ensure code quality and functionality. Use Jest or Vitest for testing.

1. Add a simple test in `client/src/__tests__/App.test.tsx`:

    ```typescript
    import { render, screen } from '@testing-library/react';
    import App from '../App';

    test('renders learn react link', () => {
      render(<App />);
      const linkElement = screen.getByText(/learn react/i);
      expect(linkElement).toBeInTheDocument();
    });
    ```

2. Run the tests:

    ```bash
    npm test
    ```

For CI/CD, integrate this into your testing pipeline using GitHub Actions or other CI tools.

---

### 5. Release

Prepare your code for release. Set up your Docker images and any necessary build steps.

1. Set up `docker-compose.yml` at the root of your project:

    ```yaml
    version: '3'
    services:
      client:
        build:
          context: ./client
        ports:
          - '3000:3000'
      server:
        build:
          context: ./server
        ports:
          - '5000:5000'
        environment:
          - MONGODB_URI=mongodb://mongo:27017/mydb
      mongo:
        image: mongo
        ports:
          - '27017:27017'
    ```

2. Build and release your images:

    ```bash
    docker-compose up --build
    ```

---

### 6. Deploy

Deploy your containers to your preferred environment, whether that’s a cloud service like AWS, Google Cloud, or DigitalOcean.

1. Push your Docker images to a registry (DockerHub, AWS ECR, etc.).
2. Deploy using your preferred platform's orchestration tools such as Kubernetes, Docker Swarm, or ECS.

---

### 7. Operate

After deploying the system, operate and manage it.

- **Monitor Logs**: Use tools like Prometheus or Grafana to monitor system health and performance.
- **CI/CD Pipelines**: Automate the entire build-test-deploy process with GitHub Actions, Jenkins, or other CI/CD tools.
  
For the server, consider setting up logging (e.g., using **Winston** or **Morgan**) and monitoring for metrics such as CPU usage, memory usage, and request time.

---

### 8. Monitor

Monitor your application to ensure it’s running smoothly.

- **Uptime Monitoring**: Use monitoring services like Pingdom or UptimeRobot to track uptime and alert you if the application goes down.
- **Performance Monitoring**: Use tools like New Relic or Datadog to track performance metrics.
- **Error Tracking**: Set up an error-tracking tool such as Sentry or LogRocket to capture and track errors in both frontend and backend applications.

---

## Final Notes

Your GitHub repository is now set up as a full DevOps-enabled template. You can clone it for new projects or share it with others. Any future enhancements or improvements can be automated through your CI/CD pipeline, ensuring a smooth development and deployment cycle.


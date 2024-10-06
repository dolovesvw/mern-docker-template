# MERN Stack Template with Docker

## Description

This is a template repository for building a MERN stack (MongoDB, Express, React, Node.js) application, with Docker for containerization and additional integrations such as Tailwind CSS, Bootstrap, Material-UI, Webpack, TypeScript, ESLint, Vitest/Jest, React Router DOM, and Auth0.

## Features

- **React** with TypeScript for the frontend.
- **Express** with TypeScript for the backend.
- **MongoDB** as the database.
- **Docker** for containerization.
- **Tailwind CSS**, **Bootstrap**, and **Material-UI** for styling.
- **Webpack** as the module bundler.
- **React Testing Library** for testing React components.
- **ESLint** for code linting and formatting.
- **Vitest/Jest** for testing and test coverage.
- **Auth0** integration for authentication.

## Getting Started

### Step 1: Clone the Repository
```bash
git clone https://github.com/yourusername/mern-docker-template.git
cd mern-docker-template
```

### Step 2: Set Up the Project Structure
```bash
mkdir client server
cd client
npx create-react-app . --template typescript
cd ../server
npm init -y
```

### Step 3: Install Dependencies
#### Client (React + TypeScript)

1. Navigate to the client directory and install the necessary dependencies:
```bash
cd client
npm install axios react-router-dom @auth0/auth0-react @mui/material @emotion/react @emotion/styled tailwindcss bootstrap
npm install --save-dev eslint jest @testing-library/react @testing-library/jest-dom
```

2. Set up Tailwind CSS:
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

5. Set up ESLint:
```bash
npx eslint --init
```

6. Add a simple test in `src/__tests__/App.test.tsx`:
```typescript
import { render, screen } from '@testing-library/react';
import App from '../App';

test('renders learn react link', () => {
  render(<App />);
  const linkElement = screen.getByText(/learn react/i);
  expect(linkElement).toBeInTheDocument();
});
```

#### Server (Node.js + Express)
1. Navigate to the server directory and install the necessary packages:
```bash
cd ../server
npm install express mongoose cors dotenv
npm install --save-dev typescript @types/node @types/express ts-node
```

2. Create a basic server setup in `src/index.ts`:
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

3. Create a *tsconfig.json* file for TypeScript:
```bash
npx tsc --init
```

4. Create a Dockerfile in the `server` directory:

```Dockerfile
FROM node:14
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
CMD ["ts-node", "src/index.ts"]
```

### Step 4: Add Docker Compose
Create a `docker-compose.yml` file at the root of the project:

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

### Step 5: Run Docker Compose
To start the application, run:
```bash
docker-compose up --build
```
- Access the client at [http://localhost:3000]
- Access the server at [http://localhost:5000]

### Step 6: Push to GitHub
1. Add and commit your changes:
```bash
git add .
git commit -m "Initial commit: MERN stack template setup"
```

2. Push to GitHub:
```bash
git push origin main
```

### Final Notes
Your GitHub repository is now set up as a template! You can clone it for new projects or share it with others. If you want to enhance the template later, just push the changes, and they'll be available for others who clone your repository.

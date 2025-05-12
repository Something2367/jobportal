# Job Portal

This repository contains a full-stack job portal application built with a React frontend, Node.js backend, and MongoDB database. The application allows users to browse and apply for jobs, while administrators can manage job postings, companies, and applicants.

## Features

### Frontend
- Built with React and Vite for fast development and performance.
- TailwindCSS for styling.
- Redux for state management.
- Components for user authentication, job browsing, and profile management.

### Backend
- Node.js with Express.js for API development.
- MongoDB for data storage.
- Authentication and authorization middleware.
- RESTful APIs for managing users, jobs, companies, and applications.

### DevOps
- Dockerized for containerized deployment.
- Kubernetes manifests for deployment to a Kubernetes cluster.
- Jenkins pipeline for CI/CD.

## Project Structure

```
jobportal/
├── backend/                # Backend code
│   ├── controllers/        # API controllers
│   ├── middlewares/        # Middleware functions
│   ├── models/             # Mongoose models
│   ├── routes/             # API routes
│   ├── utils/              # Utility functions
│   ├── Dockerfile          # Dockerfile for backend
│   └── package.json        # Backend dependencies
├── frontend/               # Frontend code
│   ├── src/                # React source code
│   ├── public/             # Static assets
│   ├── Dockerfile          # Dockerfile for frontend
│   └── package.json        # Frontend dependencies
├── k8s/                    # Kubernetes manifests
├── Jenkinsfile             # Jenkins pipeline configuration
└── docker-compose.yml      # Docker Compose configuration
```

## Prerequisites

- Node.js (v16 or higher)
- Docker
- Kubernetes cluster (for deployment)
- Jenkins (for CI/CD)

## Setup Instructions

### Local Development

1. Clone the repository:
   ```bash
   git clone https://github.com/your-repo/jobportal.git
   cd jobportal
   ```

2. Install dependencies for both frontend and backend:
   ```bash
   cd backend && npm install
   cd ../frontend && npm install
   ```

3. Start the backend server:
   ```bash
   cd backend
   npm start
   ```

4. Start the frontend development server:
   ```bash
   cd frontend
   npm run dev
   ```

5. Access the application at `http://localhost:3000`.

### Docker Deployment

1. Build Docker images:
   ```bash
   docker-compose build
   ```

2. Start the containers:
   ```bash
   docker-compose up
   ```

3. Access the application at `http://localhost`.

### Kubernetes Deployment

1. Update the Docker images in the Kubernetes manifests:
   ```bash
   sed -i "s|image: .*backend.*|image: <your-backend-image>|g" k8s/backend-deployment.yaml
   sed -i "s|image: .*frontend.*|image: <your-frontend-image>|g" k8s/frontend-deployment.yaml
   ```

2. Apply the Kubernetes configurations:
   ```bash
   kubectl apply -f k8s/
   ```

3. Access the application via the NodePort service.

### CI/CD with Jenkins

1. Configure Jenkins with the provided `Jenkinsfile`.
2. Set up environment variables for Docker registry and Kubernetes cluster.
3. Trigger the pipeline to build, test, and deploy the application.

## API Endpoints

### User
- `POST /api/v1/user/signup` - User registration
- `POST /api/v1/user/login` - User login

### Job
- `GET /api/v1/job` - Get all jobs
- `POST /api/v1/job` - Create a new job (Admin only)

### Company
- `GET /api/v1/company` - Get all companies
- `POST /api/v1/company` - Register a new company (Admin only)

### Application
- `POST /api/v1/application` - Apply for a job
- `GET /api/v1/application` - Get all applications (Admin only)

## License

This project is licensed under the MIT License. See the LICENSE file for details.

## Contributing

Contributions are welcome! Please open an issue or submit a pull request for any improvements or bug fixes.

## Acknowledgments

- [React](https://reactjs.org/)
- [Node.js](https://nodejs.org/)
- [MongoDB](https://www.mongodb.com/)
- [TailwindCSS](https://tailwindcss.com/)

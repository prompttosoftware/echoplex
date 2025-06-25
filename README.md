# echoplex

## Real-time Event Planning Platform

EchoPlex is a modern, real-time event planning platform designed to simplify event management for organizers and provide a seamless experience for attendees. From event creation and promotion to registration, ticketing, and attendance tracking, EchoPlex aims to be an all-in-one solution. The platform emphasizes a user-friendly interface, robust analytics, and secure integration with popular payment gateways.

---

## Table of Contents

1.  [Features](#features)
2.  [Getting Started](#getting-started)
    *   [Prerequisites](#prerequisites)
    *   [Local Development Setup](#local-development-setup)
3.  [System Architecture](#system-architecture)
4.  [Core Modules](#core-modules)
5.  [Testing](#testing)
6.  [Deployment](#deployment)
7.  [Monitoring & Logging](#monitoring--logging)
8.  [Technology Stack](#technology-stack)
9.  [Contributing](#contributing)
10. [License](#license)

---

## 1. Features

EchoPlex provides a comprehensive set of features for both event organizers and attendees:

*   **User Management:**
    *   Account registration and authentication (Organizer & Attendee roles).
    *   Secure user profiles with role-based access control (RBAC).
*   **Event Management:**
    *   Organizers can create, edit, publish/unpublish, and delete events.
    *   Detailed event information: name, description, date/time, location, capacity, categories, images.
    *   Attendees can browse, search, and view event details.
    *   Real-time event status updates (e.g., "Sold Out", "Cancelled").
*   **Ticketing & Registration:**
    *   Define multiple ticket types per event (e.g., VIP, General Admission, Early Bird) with custom prices, quantities, and sales periods.
    *   Seamless event registration for attendees.
    *   Real-time tracking of remaining ticket quantities.
    *   Unique registration confirmation codes.
*   **Payment Processing:**
    *   Secure credit card payments powered by Stripe integration.
    *   Support for payment initiation, success/failure handling, and transaction recording.
*   **Event Analytics:**
    *   Dashboards for organizers to track key event metrics: registrations over time, ticket sales by type, attendee counts.

---

## 2. Getting Started

Follow these instructions to get a copy of the project up and running on your local machine for development and testing purposes.

### Prerequisites

Before you begin, ensure you have the following installed:

*   **Git:** For cloning the repository.
*   **Docker & Docker Compose:** For running the application's services (database, backend, frontend) in isolated containers.

### Local Development Setup

1.  **Clone the repository:**

    ```bash
    git clone https://github.com/your-username/echoplex.git
    cd echoplex
    ```

2.  **Configure Environment Variables:**
    Create a `.env` file in the project root based on `env.example` and fill in the necessary details, especially for `DATABASE_URL` and `STRIPE_SECRET_KEY` (use Stripe test keys for local development).

    ```
    # Example .env content
    DATABASE_URL="postgresql://user:password@localhost:5432/echoplex_db"
    JWT_SECRET="your_secret_key_for_jwt"
    STRIPE_SECRET_KEY="sk_test_..."
    STRIPE_WEBHOOK_SECRET="whsec_..." # For local testing with Stripe CLI
    # Other variables as needed
    ```

3.  **Start Services with Docker Compose:**
    This command will build the Docker images (if not already built) and start all services (PostgreSQL, Backend, Frontend).

    ```bash
    docker-compose up --build
    ```

    *   The **backend** API will be accessible at `http://localhost:3000`.
    *   The **frontend** application will be accessible at `http://localhost:8080`.
    *   The **PostgreSQL** database will be running on `localhost:5432`.

4.  **Database Migrations (if applicable):**
    If the backend requires database migrations, you might need to run them manually or they will be applied automatically on startup depending on the configuration.

    *(Specific instructions will be added here once the backend framework is established for running migrations manually outside the container, e.g., `docker-compose exec backend npm run typeorm migration:run`)*

5.  **Access the Application:**
    Open your web browser and navigate to `http://localhost:8080` to interact with the EchoPlex platform.

---

## 3. System Architecture

EchoPlex is built with a modularized monolithic architecture for streamlined initial development, with clear separation of concerns to allow for future decomposition into microservices if scalability demands evolve.

*   **Frontend:** React.js Single Page Application (SPA)
*   **Backend:** Node.js with NestJS framework (provides a structured, modular, and scalable API).
*   **Database:** PostgreSQL (for robust transactional data, strong consistency, and reliability).
*   **Real-time Communication:** WebSocket layer (e.g., Socket.io) for live updates (e.g., ticket availability changes).
*   **Payment Gateway:** Stripe API for secure and seamless payment processing.
*   **Containerization:** Docker for consistent development and deployment environments.
*   **Cloud Provider:** AWS for scalable infrastructure services.

---

## 4. Core Modules

The EchoPlex platform is structured around several core functional modules:

*   **User Management:** Handles user registration, authentication (JWT), profiles, and role-based access control. Implements secure password hashing (Bcrypt).
*   **Event Management:** Provides CRUD (Create, Read, Update, Delete) operations for event organizers to manage their events and allows attendees to browse events.
*   **Ticketing & Registration:** Manages ticket types, quantities, pricing, and the entire event registration process, ensuring transactional integrity for ticket purchases.
*   **Payment Processing:** Integrates with Stripe for handling credit card payments, including `PaymentIntent` creation and webhook processing.
*   **Event Analytics:** Offers basic reporting and dashboards for organizers to view key metrics related to their events and ticket sales.

---

## 5. Testing

EchoPlex employs a comprehensive testing strategy across multiple levels to ensure reliability and quality:

*   **Unit Tests:**
    *   **Backend:** Jest (for Node.js/NestJS functions and services), React Testing Library (for isolated React components).
    *   Focus on isolated logic, pure functions, and simple component rendering.
    *   Mocking is used for external dependencies (e.g., database calls, external APIs).
*   **Integration Tests:**
    *   **Backend:** Supertest (for API endpoint testing), Testcontainers (for ephemeral, real PostgreSQL instances).
    *   Verifies interactions between services, database access, and API routing.
    *   Focuses on ensuring components work correctly together.
*   **End-to-End (E2E) Tests:**
    *   **Frontend-driven:** Cypress or Playwright (TBD).
    *   Simulates critical user journeys (e.g., event creation, ticket purchase) through the UI.
    *   Ensures the entire system functions as expected from a user's perspective.

**Test Data Management:** Utilizes inline data for unit tests, database seeders, and transactional rollbacks for integration tests, and API calls/seeding for E2E tests to ensure isolated and consistent test environments.

---

## 6. Deployment

The production environment for EchoPlex is hosted on **AWS** and utilizes the following services:

*   **Compute:** AWS ECS Fargate (serverless containers for backend and frontend).
*   **Database:** AWS RDS for PostgreSQL (managed database service).
*   **Networking:** AWS VPC, Application Load Balancer (ALB), Route 53 (DNS).
*   **Storage:** AWS S3 (for static assets like the frontend build).
*   **Security:** AWS IAM for access control, AWS WAF for web application firewall.
*   **CI/CD:** GitHub Actions for automated testing, Docker image building/pushing to AWS ECR, and automated deployments to AWS Fargate.

Deployment strategy focuses on minimal downtime through Blue/Green deployments or rolling updates.

---

## 7. Monitoring & Logging

Robust monitoring and logging are crucial for maintaining the health and performance of EchoPlex:

*   **Logging:** Structured logging (JSON format) using Winston (Node.js) forwarded to AWS CloudWatch Logs for centralized access and analysis.
*   **Monitoring:**
    *   **Application Metrics:** Prometheus with Grafana for detailed dashboards (request rates, error rates, response times) from `prom-client` within the backend.
    *   **Infrastructure Metrics:** AWS CloudWatch for monitoring ECS, RDS, and ALB.
*   **Alerting:** CloudWatch Alarms integrated with SNS for proactive notifications on critical events (e.g., high error rates, service downtime).
*   **Health Checks:** A dedicated `/health` endpoint is provided for load balancers and container orchestration to verify service availability.

---

## 8. Technology Stack

*   **Backend:**
    *   Node.js
    *   NestJS (Framework)
    *   PostgreSQL (Database)
    *   TypeORM / Prisma (ORM - TBD)
    *   Bcrypt (Password Hashing)
    *   jsonwebtoken (JWT)
    *   Socket.io (Real-time Communication)
    *   Stripe SDK (Payment Gateway)
    *   Winston (Logging)
*   **Frontend:**
    *   React (Library)
    *   React Router (Routing)
    *   Redux / Zustand (State Management - TBD)
    *   Axios (HTTP Client)
    *   Styled Components / Tailwind CSS (Styling - TBD)
    *   Socket.io-client (Real-time Communication)
*   **Tooling:**
    *   Docker
    *   Git
    *   GitHub Actions (CI/CD)
*   **Testing:**
    *   Jest (Test Runner)
    *   Supertest (API Testing)
    *   Testcontainers (Database Integration Tests)
    *   React Testing Library (Component Testing)
    *   Cypress / Playwright (E2E Testing - TBD)

---

## 9. Contributing

Details on how to contribute to the EchoPlex project will be provided in a `CONTRIBUTING.md` file.

---

## 10. License

This project is licensed under the MIT License - see the `LICENSE` file for details.

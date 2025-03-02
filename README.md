# CS4067-Assgt-EventBooking-i221165-HamzaBinRiaz
# CS4067 - Microservices-Based Online Event Booking Platform

This repository contains the implementation of a microservices-based Online Event Booking Platform for CS4067 - DevOps and Cloud Native course.

## Architecture Overview

The system is built using a microservices architecture to provide scalability, flexibility, and maintainability. It consists of the following components:

1. **User Service**: Manages user authentication and profiles
2. **Event Service**: Manages event listings and details
3. **Booking Service**: Handles ticket bookings and payment processing
4. **Notification Service**: Sends email notifications for booking confirmations
5. **Payment Gateway**: A mock service to simulate payment processing

The services communicate through both synchronous (REST APIs) and asynchronous (RabbitMQ) methods.

![Architecture Diagram](./docs/architecture_diagram.png)

## Communication Patterns

### Synchronous Communication (REST APIs)
- User Service → Event Service: Users retrieve available events
- User Service → Booking Service: Users create bookings
- Booking Service → Event Service: Check event availability before confirming booking
- Booking Service → Payment Gateway: Process payments

### Asynchronous Communication (RabbitMQ)
- Booking Service → Notification Service: Send booking confirmations and updates

## Technology Stack

- **User Service**: FastAPI/Express.js with PostgreSQL
- **Event Service**: Spring Boot/Node.js with MongoDB
- **Booking Service**: Flask/Express.js with PostgreSQL
- **Notification Service**: Flask/Express.js with MongoDB
- **Payment Gateway**: Flask (Mock Service)
- **Message Broker**: RabbitMQ
- **Project Management**: Jira & GitHub

## Project Structure

```
├── user-service/           # User management microservice
├── event-service/          # Event listing microservice
├── booking-service/        # Booking management microservice
├── notification-service/   # Notification handling microservice
├── payment-gateway/        # Mock payment service
├── docs/                   # Documentation and diagrams
└── .github/                # GitHub workflows and templates
```

## Setup and Installation

### Prerequisites
- Python 3.8+
- Node.js 14+ (if using Express.js)
- MongoDB
- PostgreSQL
- RabbitMQ

### Environment Variables
Each service requires specific environment variables to be set. Example templates are provided in each service directory as `.env.example` files.

### Setting Up Services

1. **Clone the repository**
   ```bash
   git clone https://github.com/your-username/CS4067-Assgt-EventBooking-RollNo-YourName-repo.git
   cd CS4067-Assgt-EventBooking-RollNo-YourName-repo
   ```

2. **Set up each service**
   
   For Python-based services:
   ```bash
   cd service-directory
   python -m venv venv
   source venv/bin/activate  # On Windows: venv\Scripts\activate
   pip install -r requirements.txt
   cp .env.example .env      # Edit .env with your configuration
   python app.py
   ```

   For Node.js-based services:
   ```bash
   cd service-directory
   npm install
   cp .env.example .env      # Edit .env with your configuration
   npm start
   ```

3. **Set up RabbitMQ**
   
   Using Docker:
   ```bash
   docker run -d --name rabbitmq -p 5672:5672 -p 15672:15672 rabbitmq:3-management
   ```
   
   Or install RabbitMQ directly from [https://www.rabbitmq.com/download.html](https://www.rabbitmq.com/download.html)

## API Documentation

### Payment Gateway Service

#### Process Payment
- **URL**: `/payments`
- **Method**: `POST`
- **Request Body**:
  ```json
  {
    "user_id": "string",
    "booking_id": "string",
    "amount": "number"
  }
  ```
- **Response**:
  ```json
  {
    "status": "success",
    "payment_id": "string",
    "payment_status": "APPROVED/DECLINED",
    "message": "string"
  }
  ```

#### Get Payment Details
- **URL**: `/payments/{payment_id}`
- **Method**: `GET`
- **Response**:
  ```json
  {
    "status": "success",
    "payment": {
      "payment_id": "string",
      "user_id": "string",
      "booking_id": "string",
      "amount": "number",
      "status": "string",
      "timestamp": "string"
    }
  }
  ```

### Event Service

#### Check Event Availability
- **URL**: `/events/{event_id}/availability`
- **Method**: `GET`
- **Response**:
  ```json
  {
    "status": "success",
    "event_id": "string",
    "event_name": "string",
    "available": "boolean",
    "available_tickets": "number",
    "total_tickets": "number"
  }
  ```

## Jira & GitHub Integration

This project uses Jira for task management and GitHub for version control. The integration between these platforms allows for seamless tracking of development progress.

### Jira Project
- **Project Name**: CS4067_EventBooking_RollNo_YourName
- **Workflow States**: To Do, In Progress, Review, Done

### GitHub Integration
- GitHub issues are synchronized with Jira tasks
- Commit messages reference Jira issues (e.g., "Implemented booking API (CS4067-123)")
- Pull requests update Jira issue status automatically

## Implementation Considerations

### Error Handling
All services implement comprehensive error handling with try-catch blocks and appropriate error responses.

### Logging
Each service maintains detailed logs with service name, function, and severity level in a structured format.

### Security
Sensitive data like API keys and database credentials are stored in environment variables, not hardcoded.

### API Documentation
All APIs are documented with request/response formats and error codes.

## Contributing

1. Create a new branch for your feature or bugfix
2. Make your changes
3. Create a pull request with a clear description of the changes
4. Reference the related Jira issue in your PR description

## License

This project is created for educational purposes as part of CS4067 course requirements.

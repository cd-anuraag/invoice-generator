# Invoice Generator

This is a full-stack web application for generating invoices with GST.
Users can log in, register, add products, generate invoices in PDF format, and view their past quotations.
The application is built using the **MongoDB**, **Express.js**, and **Node.js (MEN stack)** and is secured with **JWT** authentication.

## Features

- **User Registration**: Allows new users to register with a unique email.
- **User Login**: Existing users can log in and receive a JWT token for authentication.
- **Add Products**: Authenticated users can add products, calculate 18% GST, and generate invoices.
- **Generate PDF**: Invoices are automatically generated in PDF format using `puppeteer`.
- **View Quotations**: Users can view and download previously generated invoices.
- **Bonus Feature**: Option to generate invoices as images.

## Technologies Used

- **Backend**: Node.js, Express.js
- **Database**: MongoDB (using Mongoose)
- **Authentication**: JWT (JSON Web Tokens)
- **PDF Generation**: Puppeteer
- **Image Generation**: Puppeteer
- **Deployment**: Docker
- **Postman Collection**: Provided for easy API testing

## Table of Contents

- [Installation](#installation)
- [Environment Variables](#environment-variables)
- [API Endpoints](#api-endpoints)

## Installation

To get started with this project locally, follow these steps:

1. Clone the repository:
   ```bash
   git clone https://github.com/cd-anuraag/invoice-generator.git
   cd invoice-generator
    ```
   
2. Install the dependencies:
    ```bash
    npm install
    ```
   
3. Set up environment variables:
    - Create a `.env` file in the root directory.
    - Add the following environment variables:
        ```env
        PORT=3000
        MONGODB_URI=your uri
        JWT_SECRET=your_secret_key
        ```
      
4. Start the server:
    ```bash
    npm run start
    ```
   
#Postman Collection
https://www.postman.com/joint-operations-engineer-4869303/workspace/node-js/collection/38440005-7d9bf339-3a34-414a-9920-2741024a9655?action=share&creator=38440005

## API Endpoints

### 1. **User Registration**
- **POST** `/auth/register`
- **Description**: Registers a new user.
- **Request Body**:
  ```json
  {
    "name": "TEST",
    "email": "test@example.com",
    "password": "yourpassword"
  }
  ```
- **Response**:
  ```json
  {
    "message": "User registered successfully"
  }
  ```
- **Error Response** (Email already exists):
  ```json
  {
    "error": "Email already exists"
  }
  ```

### 2. **User Login**
- **POST** `/auth/login`
- **Description**: Authenticates a user and returns a JWT token.
- **Request Body**:
  ```json
  {
    "email": "test@example.com",
    "password": "yourpassword"
  }
  ```
- **Response**:
  ```json
  {
    "token": "your-jwt-token"
  }
  ```
- **Error Response** (Invalid credentials):
  ```json
  {
    "error": "Invalid email or password"
  }
  ```

### 3. **Add Products and Generate Invoice (PDF)**
- **POST** `/product/add`
- **Description**: Adds an array of products, calculates 18% GST, and generates a PDF invoice.
- **Authorization**: Requires JWT token in the `Authorization` header: `Bearer your-jwt-token`
- **Request Body**:
  ```json
  {
    "products": [
      {
        "name": "Product 1",
        "qty": 10,
        "rate": 100
      },
      {
        "name": "Product 2",
        "qty": 5,
        "rate": 200
      }
    ]
  }
  ```
- **Response**:
  ```json
  {
    "message": "Quotation created",
    "pdf": "pdf-download-link"
  }
  ```
- **Error Response** (Invalid or missing JWT):
  ```json
  {
    "error": "Unauthorized access"
  }
  ```

### 4. **View Quotations**
- **GET** `/quotation/list`
- **Description**: Returns a list of quotations generated by the user.
- **Authorization**: Requires JWT token in the `Authorization` header: `Bearer your-jwt-token`
- **Response**:
  ```json
  [
    {
      "quotationId": "some-id",
      "date": "2024-09-18",
      "pdfLink": "download-link"
    }
  ]
  ```
- **Error Response** (Invalid or missing JWT):
  ```json
  {
    "error": "Unauthorized access"
  }
  ```

### 5. **Bonus: Generate Invoice as Image**
- **POST** `/products/add?format=image`
- **Description**: Adds products and generates the invoice as an image.
- **Authorization**: Requires JWT token in the `Authorization` header: `Bearer your-jwt-token`
- **Request Body** (Same as above for products):
  ```json
  {
    "products": [
      {
        "name": "Product 1",
        "qty": 10,
        "rate": 100
      },
      {
        "name": "Product 2",
        "qty": 5,
        "rate": 200
      }
    ]
  }
  ```
- **Response**:
  ```json
  {
    "message": "Quotation created",
    "image": "image-download-link"
  }
  ```

## Additional Libraries Used
1. bcrypt - For hashing passwords
2. jsonwebtoken - For generating JWT tokens
3. mongoose - For MongoDB object modeling
4. puppeteer - For generating PDF invoices and images


## CI/CD
1. Github Actions - For CI/CD
2. Docker - For containerization
We have used Github Actions for CI/CD. The workflow is triggered on every push to the main branch. The workflow installs the dependencies, runs the tests, and builds the Docker image. The image is then pushed to the Docker Hub.
then we call deploy the latest image from render by pulling the image from docker hub.

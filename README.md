# Notes API - RESTful Node.js Application

A simple and efficient RESTful API for managing notes built with Node.js, Express, and MongoDB.

## Table of Contents
- [Overview](#overview)
- [Features](#features)
- [Tech Stack](#tech-stack)
- [Project Structure](#project-structure)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Configuration](#configuration)
- [Running the Application](#running-the-application)
- [API Documentation](#api-documentation)
- [Testing the API](#testing-the-api)
- [Database Schema](#database-schema)
- [Error Handling](#error-handling)
- [Development Tips](#development-tips)

## Overview

This is a RESTful API application that provides a complete backend solution for managing notes. It allows users to perform CRUD (Create, Read, Update, Delete) operations on notes through HTTP endpoints. The application is built using the MVC (Model-View-Controller) architectural pattern, ensuring clean code organization and separation of concerns.

## Features

- ✅ Create new notes with title and content
- ✅ Retrieve all notes or a specific note by ID
- ✅ Update existing notes
- ✅ Delete notes
- ✅ MongoDB integration for persistent data storage
- ✅ RESTful API design following best practices
- ✅ CORS enabled for cross-origin requests
- ✅ Environment variable configuration
- ✅ Error handling and validation
- ✅ Automatic timestamp for note creation

## Tech Stack

- **Runtime Environment:** Node.js
- **Web Framework:** Express.js v5.1.0
- **Database:** MongoDB with Mongoose ODM v8.18.1
- **Middleware:**
  - CORS v2.8.5 for cross-origin resource sharing
  - Express.json() for JSON body parsing
- **Environment Management:** dotenv v17.2.2
- **Development Tool:** Nodemon v3.1.10 for auto-restarting

## Project Structure

```
node-sep-10/
├── controllers/
│   └── noteController.js    # Business logic for note operations
├── models/
│   └── Note.js              # MongoDB schema definition for notes
├── routes/
│   └── noteRoutes.js        # API endpoint definitions
├── node_modules/            # Dependencies (auto-generated)
├── .env                     # Environment variables (not tracked in git)
├── .git/                    # Git repository
├── server.js                # Main application entry point
├── package.json             # Project metadata and dependencies
├── package-lock.json        # Dependency lock file
└── README.md                # This file
```

### File Descriptions

- **server.js**: The main entry point that sets up the Express server, connects to MongoDB, and configures middleware
- **models/Note.js**: Defines the Mongoose schema for notes with validation rules
- **controllers/noteController.js**: Contains all the business logic for handling note operations
- **routes/noteRoutes.js**: Maps HTTP endpoints to controller functions

## Prerequisites

Before running this application, ensure you have the following installed:

- **Node.js** (v14 or higher) - [Download](https://nodejs.org/)
- **npm** (comes with Node.js)
- **MongoDB** - Either:
  - Local MongoDB instance - [Download](https://www.mongodb.com/try/download/community)
  - MongoDB Atlas account (cloud) - [Sign up](https://www.mongodb.com/cloud/atlas)

## Installation

1. **Clone the repository** (if using git):
   ```bash
   git clone <repository-url>
   cd node-sep-10
   ```

2. **Install dependencies**:
   ```bash
   npm install
   ```

   This will install all required packages listed in `package.json`:
   - express
   - mongoose
   - cors
   - dotenv
   - nodemon (dev dependency)

## Configuration

### Environment Variables

The application uses environment variables for configuration. Create a `.env` file in the root directory with the following variables:

```env
PORT=5000
MONGO_URI=your_mongodb_connection_string
```

#### Setting up MongoDB Connection String:

**For MongoDB Atlas (Cloud):**
1. Log in to MongoDB Atlas
2. Create a cluster (free tier available)
3. Create a database user with username and password
4. Whitelist your IP address (or allow access from anywhere for development)
5. Click "Connect" and choose "Connect your application"
6. Copy the connection string and replace `<password>` with your database user password
7. Format: `mongodb+srv://<username>:<password>@cluster.xxxxx.mongodb.net/<database-name>?retryWrites=true&w=majority`

**For Local MongoDB:**
```env
MONGO_URI=mongodb://localhost:27017/notesdb
```

## Running the Application

### Development Mode (with auto-restart)
```bash
npm run dev
```
This uses Nodemon to automatically restart the server when you make changes to the code.

### Production Mode
```bash
npm start
```
Or directly:
```bash
node server.js
```

### Expected Console Output
When the application starts successfully, you should see:
```
Connected to MongoDB
Server running on port 5000
```

## API Documentation

### Base URL
```
http://localhost:5000/api/notes
```

### Endpoints

#### 1. Get All Notes
- **Endpoint:** `GET /api/notes`
- **Description:** Retrieves all notes from the database
- **Response:** Array of note objects
- **Example Response:**
  ```json
  [
    {
      "_id": "65f8a9b2c3d4e5f6g7h8i9j0",
      "title": "My First Note",
      "content": "This is the content of my first note",
      "createdAt": "2024-03-18T10:30:00.000Z",
      "__v": 0
    }
  ]
  ```

#### 2. Get Note by ID
- **Endpoint:** `GET /api/notes/:id`
- **Description:** Retrieves a specific note by its ID
- **Parameters:** `id` - MongoDB ObjectId
- **Response:** Single note object
- **Example Request:** `GET /api/notes/65f8a9b2c3d4e5f6g7h8i9j0`
- **Example Response:**
  ```json
  {
    "_id": "65f8a9b2c3d4e5f6g7h8i9j0",
    "title": "My First Note",
    "content": "This is the content of my first note",
    "createdAt": "2024-03-18T10:30:00.000Z",
    "__v": 0
  }
  ```

#### 3. Create New Note
- **Endpoint:** `POST /api/notes`
- **Description:** Creates a new note
- **Request Body:**
  ```json
  {
    "title": "Note Title",
    "content": "Note content goes here"
  }
  ```
- **Response:** Created note object with auto-generated ID and timestamp
- **Status Code:** 201 (Created)

#### 4. Update Note
- **Endpoint:** `PUT /api/notes/:id`
- **Description:** Updates an existing note
- **Parameters:** `id` - MongoDB ObjectId
- **Request Body:**
  ```json
  {
    "title": "Updated Title",
    "content": "Updated content"
  }
  ```
- **Response:** Updated note object

#### 5. Delete Note
- **Endpoint:** `DELETE /api/notes/:id`
- **Description:** Deletes a note
- **Parameters:** `id` - MongoDB ObjectId
- **Response:**
  ```json
  {
    "message": "Note deleted successfully"
  }
  ```

## Testing the API

### Using cURL

**Create a note:**
```bash
curl -X POST http://localhost:5000/api/notes \
  -H "Content-Type: application/json" \
  -d '{"title":"Test Note","content":"This is a test note"}'
```

**Get all notes:**
```bash
curl http://localhost:5000/api/notes
```

**Get specific note:**
```bash
curl http://localhost:5000/api/notes/YOUR_NOTE_ID
```

**Update a note:**
```bash
curl -X PUT http://localhost:5000/api/notes/YOUR_NOTE_ID \
  -H "Content-Type: application/json" \
  -d '{"title":"Updated Title","content":"Updated content"}'
```

**Delete a note:**
```bash
curl -X DELETE http://localhost:5000/api/notes/YOUR_NOTE_ID
```

### Using Postman

1. Download and install [Postman](https://www.postman.com/downloads/)
2. Create a new collection called "Notes API"
3. Add requests for each endpoint:
   - Set the HTTP method (GET, POST, PUT, DELETE)
   - Enter the URL (e.g., `http://localhost:5000/api/notes`)
   - For POST and PUT requests, go to Body → raw → JSON and add the request body
   - Click Send to test the endpoint

### Using Thunder Client (VS Code Extension)

1. Install Thunder Client extension in VS Code
2. Create a new request
3. Follow similar steps as Postman

## Database Schema

### Note Model

The Note model is defined in `models/Note.js` with the following schema:

| Field | Type | Required | Default | Description |
|-------|------|----------|---------|-------------|
| `_id` | ObjectId | Auto-generated | - | Unique identifier |
| `title` | String | Yes | - | Title of the note |
| `content` | String | Yes | - | Content/body of the note |
| `createdAt` | Date | Auto-generated | Current timestamp | Creation timestamp |

## Error Handling

The application implements comprehensive error handling:

### Status Codes
- **200 OK**: Successful GET, PUT requests
- **201 Created**: Successful POST request
- **400 Bad Request**: Invalid request data or validation errors
- **404 Not Found**: Resource not found
- **500 Internal Server Error**: Server-side errors

### Error Response Format
```json
{
  "message": "Error description"
}
```

## Development Tips

### Adding New Features

1. **Add a new field to the Note model:**
   - Update `models/Note.js` with the new field
   - Update controller methods to handle the new field
   - Test with existing data

2. **Add authentication:**
   - Install JWT packages: `npm install jsonwebtoken bcrypt`
   - Create User model
   - Add auth middleware
   - Protect routes as needed

3. **Add search functionality:**
   - Add a search endpoint in routes
   - Implement text search in controller using MongoDB text indices

### Common Issues and Solutions

**Issue:** "Cannot connect to MongoDB"
- **Solution:** Check if MongoDB is running locally or verify Atlas connection string and IP whitelist

**Issue:** "Port already in use"
- **Solution:** Change the PORT in .env file or kill the process using the port

**Issue:** "Module not found"
- **Solution:** Run `npm install` to install missing dependencies

### Best Practices

1. **Always use environment variables** for sensitive data
2. **Validate input data** before saving to database
3. **Use proper HTTP status codes** in responses
4. **Implement proper error handling** with try-catch blocks
5. **Keep controllers thin** - business logic should be minimal
6. **Use async/await** for better readable asynchronous code
7. **Add input sanitization** for production use

## Scripts

Available npm scripts defined in `package.json`:

- `npm start` - Start the server in production mode
- `npm run dev` - Start the server with Nodemon for development
- `npm test` - Run tests (currently not implemented)

## Future Enhancements

Potential improvements for this application:

- [ ] Add user authentication and authorization
- [ ] Implement pagination for notes listing
- [ ] Add search and filter capabilities
- [ ] Include categories or tags for notes
- [ ] Add file attachment support
- [ ] Implement real-time updates using WebSockets
- [ ] Add unit and integration tests
- [ ] Add API rate limiting
- [ ] Implement data validation middleware
- [ ] Add logging system
- [ ] Create API documentation with Swagger

## License

ISC

## Support

For issues or questions about this application, please create an issue in the repository or contact the development team.

---

**Note:** Remember to never commit your `.env` file to version control. Always add it to `.gitignore` to keep your sensitive information secure.
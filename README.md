# Journal App

A RESTful journal application built with **Java**, **Spring Boot**, **Spring Security**, and **MongoDB**. It allows authenticated users to create, retrieve, update, and delete journal entries associated with their accounts.

## Tech Stack

- Java 8
- Spring Boot 2.7.16
- Spring Web
- Spring Security
- Spring Data MongoDB
- MongoDB
- Maven
- Lombok

## Features

- Store user accounts and journal entries in MongoDB.
- Associate journal entries with their respective users.
- Create journal entries with an automatically assigned timestamp.
- Retrieve all journal entries belonging to the authenticated user.
- Retrieve an individual journal entry by its ID.
- Update a journal entry's title and/or content.
- Delete a journal entry.
- Hash user passwords with BCrypt when users are saved through the user service.
- Use MongoDB transaction management for journal-entry operations.

## Project Structure

```text
journalApp/
├── src/
│   ├── main/
│   │   └── java/journalApp/
│   │       ├── JournalApplication.java
│   │       ├── controller/
│   │       │   ├── JournalEntryController.java
│   │       │   └── UserController.java
│   │       ├── entity/
│   │       │   ├── JournalEntry.java
│   │       │   └── User.java
│   │       ├── repository/
│   │       │   ├── JournalEntryRepository.java
│   │       │   └── UserRepository.java
│   │       └── service/
│   │           ├── JournalEntryService.java
│   │           └── UserService.java
│   └── test/
│       └── java/journalApp/
│           └── JournalAppApplicationTests.java
├── .mvn/
├── mvnw
├── mvnw.cmd
└── pom.xml
```

## Prerequisites

Make sure you have the following installed:

- Java 8 or a compatible JDK
- MongoDB
- Git (to clone the repository)

## Getting Started

### 1. Clone the repository

```bash
git clone https://github.com/Shre-coder22/journalApp.git
cd journalApp
```

### 2. Start MongoDB

Start a MongoDB instance that the application can connect to.

The repository does not include an application configuration file, so configure the MongoDB connection using Spring Boot's `spring.data.mongodb.uri` property if your MongoDB instance is not available at the default local connection.

For example, you can set the environment variable:

```bash
export SPRING_DATA_MONGODB_URI="mongodb://localhost:27017/journaldb"
```

On Windows PowerShell:

```powershell
$env:SPRING_DATA_MONGODB_URI="mongodb://localhost:27017/journaldb"
```

### 3. Run the application

On macOS/Linux:

```bash
./mvnw spring-boot:run
```

On Windows:

```powershell
.\mvnw.cmd spring-boot:run
```

The application uses Spring Boot's default server port, **8080**.

## API Endpoints

The journal endpoints use the authenticated username to determine which user's entries are accessed.

| Method | Endpoint | Description |
|---|---|---|
| `GET` | `/journal` | Retrieve all journal entries belonging to the authenticated user |
| `POST` | `/journal` | Create a journal entry |
| `GET` | `/journal/id/{myId}` | Retrieve a journal entry by its MongoDB ObjectId |
| `PUT` | `/journal/id/{myId}` | Update a journal entry's title and/or content |
| `DELETE` | `/journal/id/{myId}` | Delete a journal entry |
| `PUT` | `/user` | Update the authenticated user's username and password |
| `DELETE` | `/user/user` | Delete the authenticated user's account |

### Example: Create a journal entry

**Request**

```http
POST /journal
Content-Type: application/json
```

**JSON body**

```json
{
  "title": "My First Entry",
  "content": "Today I started using my journal app."
}
```

The application assigns the entry's date when it is saved.

### Example: Update a journal entry

**Request**

```http
PUT /journal/id/{myId}
Content-Type: application/json
```

**JSON body**

```json
{
  "title": "Updated Title",
  "content": "Updated journal content."
}
```

The update operation changes the supplied non-empty fields.

## Data Model

### User

A user document contains:

- `id`: MongoDB ObjectId
- `username`: Unique username
- `password`: BCrypt-hashed password when saved through the user-creation service methods
- `journalEntries`: References to the user's journal entries
- `roles`: User roles

### Journal Entry

A journal-entry document contains:

- `id`: MongoDB ObjectId
- `title`: Entry title
- `content`: Entry content
- `date`: Date and time when the entry is saved

## Running Tests

Run the test suite with Maven:

```bash
./mvnw test
```

On Windows:

```powershell
.\mvnw.cmd test
```

## Notes

- Spring Security is included, but the repository does not contain a custom security configuration. Spring Boot's default security auto-configuration therefore applies.
- The repository currently exposes journal-entry and user-management endpoints; it does not include a dedicated user-registration endpoint or a frontend.
- Passwords are hashed with BCrypt in the user service. Journal-entry content is not encrypted by the implementation in this repository.

## License

No license file is currently included in this repository.

# **Assignment 8: Library Management System API**

**Objective:**  
Develop a backend API for a Library Management System that allows library staff and members to manage books, authors, memberships, and borrowing activities. The API will be structured around CRUD operations for books, authors, members, and borrow records, with additional endpoints for borrowing and returning books. You will use UUID for unique identification in all tables.

---

## **Technologies Required**
- **Prisma ORM**
- **Node.js**
- **PostgreSQL**
- **Express.js**
- **TypeScript**

---

## **Database Schema Requirements**

Use Prisma ORM to design the following schema, ensuring each table uses UUID as a primary key:

### **1. Book Table**
| Field            | Type      | Description                                      |
|------------------|-----------|--------------------------------------------------|
| `bookId`         | UUID      | Unique identifier for each book                  |
| `title`          | String    | Title of the book                                |
| `genre`          | String    | Genre or category of the book                    |
| `publishedYear`  | Int       | Year the book was published                      |
| `totalCopies`    | Int       | Total copies of the book in the library          |
| `availableCopies`| Int       | Number of copies currently available for borrowing |

### **2. Author Table**
| Field            | Type      | Description                                      |
|------------------|-----------|--------------------------------------------------|
| `authorId`       | UUID      | Unique identifier for each author                |
| `name`           | String    | Name of the author                               |
| `bio`            | Text      | Short biography of the author                    |
| `dateOfBirth`    | DateTime  | Birth date of the author                         |


### **3. Member Table**
| Field            | Type      | Description                                      |
|------------------|-----------|--------------------------------------------------|
| `memberId`       | UUID      | Unique identifier for each member                |
| `name`           | String    | Name of the library member                       |
| `email`          | String    | Member’s email (unique)                          |
| `phone`          | String    | Member’s contact number                          |
| `membershipDate` | DateTime  | Date the member joined the library               |


### **4. BorrowRecord Table**
| Field            | Type      | Description                                      |
|------------------|-----------|--------------------------------------------------|
| `borrowId`       | UUID      | Unique identifier for each borrow record         |
| `borrowDate`     | DateTime  | Date when the book was borrowed                  |
| `returnDate`     | DateTime  | Date when the book was returned (nullable)       |
| `bookId`         | UUID      | Foreign key referencing `Book`                   |
| `memberId`       | UUID      | Foreign key referencing `Member`                 |

---

## **Features & Endpoints**

### **Main Section (50 Marks)**

#### **1. Book CRUD Operations**

- **Create Book**  
Creates a new book record in the library’s database.

  **Endpoint:** `POST /api/books`
  
  **Request Body:**
  ```json
  {
    "title": "To Kill a Mockingbird",
    "genre": "Fiction",
    "publishedYear": 1960,
    "totalCopies": 10,
    "availableCopies": 10
  }
  ```
  
  **Response:**
  ```json
  {
    "success": true,
    "status": 201,
    "message": "Book created successfully",
    "data": {
      "bookId": "a24df67b-1234-5678-9101-b2a3c5d7f9b1",
      "title": "To Kill a Mockingbird",
      "genre": "Fiction",
      "publishedYear": 1960,
      "totalCopies": 10,
      "availableCopies": 10
    }
  }
  ```

- **Read All Books**  
Retrieves a list of all books in the library.

  **Endpoint:** `GET /api/books`
  
  **Response:**
  ```json
  {
    "success": true,
    "status": 200,
    "message": "Books retrieved successfully",
    "data": [
      {
        "bookId": "a24df67b-1234-5678-9101-b2a3c5d7f9b1",
        "title": "To Kill a Mockingbird",
        "genre": "Fiction",
        "publishedYear": 1960,
        "totalCopies": 10,
        "availableCopies": 10
      }
    ]
  }
  ```

- **Read Book by ID**  
Fetches details of a specific book by its `bookId`.

  **Endpoint:** `GET /api/books/:bookId`
  
  **Response:**
  ```json
  {
    "success": true,
    "status": 200,
    "message": "Book retrieved successfully",
    "data": {
      "bookId": "a24df67b-1234-5678-9101-b2a3c5d7f9b1",
      "title": "To Kill a Mockingbird",
      "genre": "Fiction",
      "publishedYear": 1960,
      "totalCopies": 10,
      "availableCopies": 10
    }
  }
  ```

- **Update Book**  
Updates information of an existing book by its `bookId`.

  **Endpoint:** `PUT /api/books/:bookId`
  
  **Request Body:**
  ```json
  {
    "title": "To Kill a Mockingbird - Revised",
    "genre": "Classic Fiction",
    "publishedYear": 1961,
    "totalCopies": 12,
    "availableCopies": 8
  }
  ```
  
  **Response:**
  ```json
  {
    "success": true,
    "status": 200,
    "message": "Book updated successfully",
    "data": {
      "bookId": "a24df67b-1234-5678-9101-b2a3c5d7f9b1",
      "title": "To Kill a Mockingbird - Revised",
      "genre": "Classic Fiction",
      "publishedYear": 1961,
      "totalCopies": 12,
      "availableCopies": 8
    }
  }
  ```

- **Delete Book**  
Deletes a book from the library by its `bookId`.

  **Endpoint:** `DELETE /api/books/:bookId`
  
  **Response:**
  ```json
  {
    "success": true,
    "status": 200,
    "message": "Book successfully deleted"
  }
  ```

---

#### **2. Member CRUD Operations**

- **Create Member**  
Adds a new member to the library.

  **Endpoint:** `POST /api/members`
  
  **Request Body:**
  ```json
  {
    "name": "Alice Johnson",
    "email": "alice@example.com",
    "phone": "123-456-7890",
    "membershipDate": "2024-01-01T00:00:00.000Z"
  }
  ```
  
  **Response:**
  ```json
  {
    "success": true,
    "status": 201,
    "message": "Member created successfully",
    "data": {
      "memberId": "8765-4321-1098",
      "name": "Alice Johnson",
      "email": "alice@example.com",
      "phone": "123-456-7890",
      "membershipDate": "2024-01-01T00:00:00.000Z"
    }
  }
  ```

- **Read All Members**  
Retrieves a list of all members in the library.

  **Endpoint:** `GET /api/members`
  
  **Response:**
  ```json
  {
    "success": true,
    "status": 200,
    "message": "Members retrieved successfully",
    "data": [
      {
        "memberId": "8765-4321-1098",
        "name": "Alice Johnson",
        "email": "alice@example.com",
        "phone": "123-456-7890",
        "membershipDate": "2024-01-01T00:00:00.000Z"
      }
    ]
  }
  ```

- **Read Member by ID**  
Fetches details of a specific member by their `memberId`.

  **Endpoint:** `GET /api/members/:memberId`
  
  **Response:**
  ```json
  {
    "success": true,
    "status": 200,
    "message": "Member retrieved successfully",
    "data": {
      "memberId": "8765-4321-1098",
      "name": "Alice Johnson",
      "email": "alice@example.com",
      "phone": "123-456-7890",
      "membershipDate": "2024-01-01T00:00:00.000Z"
    }
  }
  ```

- **Update Member**  
Updates information for a specific member by their `memberId`.

  **Endpoint:** `PUT /api/members/:memberId`
  
  **Request Body:**
  ```json
  {
    "name": "Alice Johnson",
    "email": "alice.johnson@example.com",
    "phone": "098-765-4321"
  }
  ```
  
  **Response:**
  ```json
  {
    "success": true,
    "status": 200,
    "message": "Member updated successfully",
    "data": {
      "memberId": "8765-4321-1098",
      "name": "Alice Johnson",
      "email": "alice.johnson@example.com",
      "phone": "098-765-4321"
    }
  }
  ```

- **Delete Member**  
Deletes a member from the library by their `memberId`.

  **Endpoint:** `DELETE /api/members/:memberId`
  
  **Response:**
  ```json
  {
    "success": true,
    "status": 200,
    "message": "Member successfully deleted"
  }
  ```

---

### **3. Borrow & Return Books**

#### **Borrow a Book**
- **Endpoint:** `POST /api/borrow`
- **Request Body:**
  ```json
  {
    "bookId": "a24df67b-1234-5678-9101-b2a3c5d7f9b1",
    "memberId": "a24df67b-1234-5678-9101-b2a3c5d7f098"
  }
  ```
- **Response:**
  ```json
  {
    "success": true,
    "status": 200,
    "message": "Book borrowed successfully",
    "data": {
      "borrowId": "a24df67b-1234-5678-9101-b2a3c5d7f",
      "bookId": "a24df67b-1234-5678-9101-b2a3c5d7f9b1",
      "memberId": "8765-4321-1098",
      "borrowDate": "2024-09-01T10:00:00.000Z"
    }
  }
  ```

#### **Return a Book**
- **Endpoint:** `POST /api/return`
- **Request Body:**
  ```json
  {
    "borrowId": "a24df67b-1234-5678-9101-b2a3c5d7f"
  }
  ```
- **Response:**
  ```json
  {
    "success": true,
    "status": 200,
    "message": "Book returned successfully"
  }
  ```

---

## **Bonus Section (10 Marks)**

### **1. Error Handling for Library System**

When an error occurs in the system, the following structure will be used in the response:

```json
{
  "success": false,
  "status": <HTTP_STATUS_CODE>,
  "message": "<Error message>",
}
```

#### **Explanation**:
- **success**: Always set to `false` in case of an error to indicate failure.
- **status**: The HTTP status code that corresponds to the error (e.g., 404 for not found, 400 for bad request, 500 for server errors).
- **message**: A short description of the error, providing a general overview of what went wrong (e.g., "Invalid book ID", "No available copies").



### **2. Overdue Borrow List**

- **Description**: Track overdue borrowed books. Admins can view a list of books that are past the due date for return.
  
- **Functionality**: 
  - Calculate overdue books based on the due date.
  - Provide a list of overdue items with borrower details.

- **Response**:
  - If overdue books exist:
    ```json
    {
      "success": true,
      "status": 200,
      "message": "Overdue borrow list fetched",
      "data": [
        {
          "borrowId": "b1234",
          "bookTitle": "To Kill a Mockingbird",
          "borrowerName": "John Doe",
          "overdueDays": 5
        }
      ]
    }
    ```
  - If no overdue books:
    ```json
    {
      "success": true,
      "status": 200,
      "message": "No overdue books",
      "data": []
    }
    ```

---

Sure, here’s a revised version of the submission guidelines:

---

## **Submission Guidelines:**

- **Codebase:** Ensure your codebase is clean, well-organized, and well-documented. Every key section of your code should have comments explaining its functionality clearly.
  
- **README File:** Your project must include a comprehensive README file that covers the following:
  - **Project Name & Description:** Provide a brief overview of what the project is and its purpose.
  - **Live URL:** Link to the live deployment of your backend.
  - **Technology Stack & Packages:** List all the technologies and packages used in the project.
  - **Setup Instructions:** Provide clear steps for installation and running the application.
  - **Key Features & Functionality:** Highlight the main features and functionality of your project.
  - **Known Issues/Bugs:** Mention any unresolved issues or bugs.
  - **Professional Formatting:** The README should be neatly formatted and easy to navigate.

---


## **What You Need to Submit:**

1. **Live Deployment Link (Backend):** Provide the URL for the live backend of your project. You can deploy your backend using [Superbase](https://www.supabase.com) or any other platform of your choice.
   
2. **GitHub Repository Link:** Share the link to your GitHub repository where the full code for your project is hosted.

---


## **Deadline:**

- **60 Marks:** [13 Nov, 2024, 11:59 PM]
- **50 Marks:** [14 Nov, 2024, 11:59 PM]
- After **14 Nov, 2024**, 30 Marks Deadline.

---

## **Important Notes:**

- **Plagiarism Policy:** Plagiarism will result in 0 marks. Ensure all code submitted is your own work.
  
- **Commit History Requirement:** Your repository must show at least 10 meaningful commits throughout the development process, demonstrating continuous progress.

---

### **By adhering to these guidelines, you’ll be on track to successfully complete Assignment 8. Good luck! 🍀**

---
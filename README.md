# SQLTASK-1
This is a beginner-friendly project that demonstrates how to create and manage a Library Management System using MySQL and DBeaver. The database keeps track of authors, books, categories, library members, and book loans.

🧾 Overview
This project includes the following six main tables:

Table Name	Description
authors	Stores information about book authors
books	Stores book details and links each book to a category
categories	Stores different categories of books
members	Stores information about library members
loans	Tracks which member borrowed which book and when it was returned
bookauthor	Junction table to manage many-to-many relationships between books and authors

The relationships between tables are managed using foreign keys. For example:

Each book has a category_id that refers to the categories table.

The bookauthor table links books to one or more authors.

The loans table references both book_id and member_id.

🛠 How to Set It Up
✅ Step 1: Create and Select the Database
sql
Copy
Edit
DROP DATABASE IF EXISTS LibraryDB;
CREATE DATABASE LibraryDB;
USE LibraryDB;
✅ Step 2: Create the Tables
sql
Copy
Edit
-- Categories Table
CREATE TABLE Categories (
    CategoryID INT AUTO_INCREMENT PRIMARY KEY,
    CategoryName VARCHAR(50) UNIQUE NOT NULL
);

-- Members Table
CREATE TABLE Members (
    MemberID INT AUTO_INCREMENT PRIMARY KEY,
    Name VARCHAR(100) NOT NULL,
    Email VARCHAR(100) UNIQUE NOT NULL,
    MembershipDate DATE NOT NULL
);

-- Authors Table
CREATE TABLE Authors (
    AuthorID INT AUTO_INCREMENT PRIMARY KEY,
    AuthorName VARCHAR(100) NOT NULL
);

-- Books Table
CREATE TABLE Books (
    BookID INT AUTO_INCREMENT PRIMARY KEY,
    Title VARCHAR(200) NOT NULL,
    ISBN VARCHAR(20) UNIQUE NOT NULL,
    CategoryID INT,
    FOREIGN KEY (CategoryID) REFERENCES Categories(CategoryID)
);

-- BookAuthor Table (junction table)
CREATE TABLE BookAuthor (
    BookID INT,
    AuthorID INT,
    PRIMARY KEY (BookID, AuthorID),
    FOREIGN KEY (BookID) REFERENCES Books(BookID) ON DELETE CASCADE,
    FOREIGN KEY (AuthorID) REFERENCES Authors(AuthorID) ON DELETE CASCADE
);

-- Loans Table
CREATE TABLE Loans (
    LoanID INT AUTO_INCREMENT PRIMARY KEY,
    BookID INT NOT NULL,
    MemberID INT NOT NULL,
    LoanDate DATE NOT NULL,
    ReturnDate DATE,
    FOREIGN KEY (BookID) REFERENCES Books(BookID),
    FOREIGN KEY (MemberID) REFERENCES Members(MemberID)
);

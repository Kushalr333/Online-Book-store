<img width="648" height="455" alt="image" src="https://github.com/user-attachments/assets/38a684e1-350f-4fd4-88c3-d4e6fa0b98b6" />
Here is a comprehensive README file based on the SQL project you provided.

-----

# SQL Portfolio Project: Online Bookstore Analysis

This project demonstrates proficiency in SQL by analyzing data for a fictional online bookstore. It involves creating a database, defining a schema with multiple related tables, loading data from CSV files, and executing a series of analytical queries. The queries range from basic data retrieval to more advanced analysis involving joins, aggregations, and subqueries.

## Database Schema

The database, `OnlineBookstore`, consists of three tables:

### 1\. `Books`

Stores information about the books available in the store.

  * **Book\_ID** (SERIAL PRIMARY KEY): Unique identifier for each book.
  * **Title** (VARCHAR): Title of the book.
  * **Author** (VARCHAR): Author of the book.
  * **Genre** (VARCHAR): Genre of the book.
  * **Published\_Year** (INT): Year the book was published.
  * **Price** (NUMERIC): Price of the book.
  * **Stock** (INT): Current stock quantity.

### 2\. `Customers`

Stores information about the customers.

  * **Customer\_ID** (SERIAL PRIMARY KEY): Unique identifier for each customer.
  * **Name** (VARCHAR): Customer's name.
  * **Email** (VARCHAR): Customer's email address.
  * **Phone** (VARCHAR): Customer's phone number.
  * **City** (VARCHAR): City where the customer resides.
  * **Country** (VARCHAR): Country where the customer resides.

### 3\. `Orders`

Stores information about book orders placed by customers.

  * **Order\_ID** (SERIAL PRIMARY KEY): Unique identifier for each order.
  * **Customer\_ID** (INT, FOREIGN KEY): References `Customer_ID` in the `Customers` table.
  * **Book\_ID** (INT, FOREIGN KEY): References `Book_ID` in the `Books` table.
  * **Order\_Date** (DATE): Date the order was placed.
  * **Quantity** (INT): Number of books purchased in the order.
  * **Total\_Amount** (NUMERIC): Total cost of the order.
<img width="1672" height="862" alt="image" src="https://github.com/user-attachments/assets/5dde9768-973e-47de-9ca4-87a3c58cc784" />

## Setup & Installation

1.  **Create Database:**
    ```sql
    CREATE DATABASE OnlineBookstore;
    ```
2.  **Switch to Database:** (Using `psql` command)
    ```sql
    \c OnlineBookstore;
    ```
3.  **Create Tables:**
    ```sql
    CREATE TABLE Books (
        Book_ID SERIAL PRIMARY KEY,
        Title VARCHAR(100),
        Author VARCHAR(100),
        Genre VARCHAR(50),
        Published_Year INT,
        Price NUMERIC(10, 2),
        Stock INT
    );

    CREATE TABLE Customers (
        Customer_ID SERIAL PRIMARY KEY,
        Name VARCHAR(100),
        Email VARCHAR(100),
        Phone VARCHAR(15),
        City VARCHAR(50),
        Country VARCHAR(150)
    );

    CREATE TABLE Orders (
        Order_ID SERIAL PRIMARY KEY,
        Customer_ID INT REFERENCES Customers(Customer_ID),
        Book_ID INT REFERENCES Books(Book_ID),
        Order_Date DATE,
        Quantity INT,
        Total_Amount NUMERIC(10, 2)
    );
    ```
4.  **Import Data:**
    *Note: This step requires you to have the `Books.csv`, `Customers.csv`, and `Orders.csv` files. You must update the file path to match the location on your local machine.*
    ```sql
    COPY Books(Book_ID, Title, Author, Genre, Published_Year, Price, Stock) 
    FROM 'D:\path\to\your\Books.csv' 
    CSV HEADER;

    COPY Customers(Customer_ID, Name, Email, Phone, City, Country) 
    FROM 'D:\path\to\your\Customers.csv' 
    CSV HEADER;

    COPY Orders(Order_ID, Customer_ID, Book_ID, Order_Date, Quantity, Total_Amount) 
    FROM 'D:\path\to\your\Orders.csv' 
    CSV HEADER;
    ```

## SQL Queries & Analysis

Here are the questions and their corresponding SQL solutions used to analyze the bookstore's data.

### Basic Queries

**1) Retrieve all books in the "Fiction" genre:**

```sql
SELECT * FROM Books 
WHERE Genre='Fiction';
```

**2) Find books published after the year 1950:**

```sql
SELECT * FROM Books 
WHERE Published_year>1950;
```

**3) List all customers from the Canada:**

```sql
SELECT * FROM Customers 
WHERE country='Canada';
```

**4) Show orders placed in November 2023:**

```sql
SELECT * FROM Orders 
WHERE order_date BETWEEN '2023-11-01' AND '2023-11-30';
```

**5) Retrieve the total stock of books available:**

```sql
SELECT SUM(stock) AS Total_Stock
From Books;
```

**6) Find the details of the most expensive book:**

```sql
SELECT * FROM Books 
ORDER BY Price DESC 
LIMIT 1;
```

**7) Show all customers who ordered more than 1 quantity of a book:**

```sql
SELECT * FROM Orders 
WHERE quantity>1;
```

**8) Retrieve all orders where the total amount exceeds $20:**

```sql
SELECT * FROM Orders 
WHERE total_amount>20;
```

**9) List all genres available in the Books table:**

```sql
SELECT DISTINCT genre FROM Books;
```

**10) Find the book with the lowest stock:**

```sql
SELECT * FROM Books 
ORDER BY stock 
LIMIT 1;
```

**11) Calculate the total revenue generated from all orders:**

```sql
SELECT SUM(total_amount) As Revenue 
FROM Orders;
```

-----

### Advanced Queries

**1) Retrieve the total number of books sold for each genre:**

```sql
SELECT b.Genre, SUM(o.Quantity) AS Total_Books_sold
FROM Orders o
JOIN Books b ON o.book_id = b.book_id
GROUP BY b.Genre;
```

**2) Find the average price of books in the "Fantasy" genre:**

```sql
SELECT AVG(price) AS Average_Price
FROM Books
WHERE Genre = 'Fantasy';
```

**3) List customers who have placed at least 2 orders:**

```sql
SELECT o.customer_id, c.name, COUNT(o.Order_id) AS ORDER_COUNT
FROM orders o
JOIN customers c ON o.customer_id=c.customer_id
GROUP BY o.customer_id, c.name
HAVING COUNT(Order_id) >=2;
```

**4) Find the most frequently ordered book:**

```sql
SELECT o.Book_id, b.title, COUNT(o.order_id) AS ORDER_COUNT
FROM orders o
JOIN books b ON o.book_id=b.book_id
GROUP BY o.book_id, b.title
ORDER BY ORDER_COUNT DESC LIMIT 1;
```

**5) Show the top 3 most expensive books of 'Fantasy' Genre :**

```sql
SELECT * FROM books
WHERE genre ='Fantasy'
ORDER BY price DESC LIMIT 3;
```

**6) Retrieve the total quantity of books sold by each author:**

```sql
SELECT b.author, SUM(o.quantity) AS Total_Books_Sold
FROM orders o
JOIN books b ON o.book_id=b.book_id
GROUP BY b.Author;
```

**7) List the cities where customers who spent over $30 are located:**

```sql
SELECT DISTINCT c.city, total_amount
FROM orders o
JOIN customers c ON o.customer_id=c.customer_id
WHERE o.total_amount > 30;
```

**8) Find the customer who spent the most on orders:**

```sql
SELECT c.customer_id, c.name, SUM(o.total_amount) AS Total_Spent
FROM orders o
JOIN customers c ON o.customer_id=c.customer_id
GROUP BY c.customer_id, c.name
ORDER BY Total_spent Desc LIMIT 1;
```

**9) Calculate the stock remaining after fulfilling all orders:**

```sql
SELECT b.book_id, b.title, b.stock, COALESCE(SUM(o.quantity),0) AS Order_quantity,  
	b.stock- COALESCE(SUM(o.quantity),0) AS Remaining_Quantity
FROM books b
LEFT JOIN orders o ON b.book_id=o.book_id
GROUP BY b.book_id ORDER BY b.book_id;
```

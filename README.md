-- Question 1: Achieving 1NF (First Normal Form)
-- The Products column in ProductDetail contains multiple values, violating 1NF.
-- To achieve 1NF, we create a new table where each row represents a single product for an order.
-- We'll split the comma-separated Products into individual rows.

CREATE TABLE ProductDetail_1NF (
    OrderID INT,
    CustomerName VARCHAR(50),
    Product VARCHAR(50),
    PRIMARY KEY (OrderID, Product) -- Composite key to ensure uniqueness
);

-- Insert data into the 1NF table, splitting Products into separate rows
INSERT INTO ProductDetail_1NF (OrderID, CustomerName, Product) VALUES
(101, 'John Doe', 'Laptop'),
(101, 'John Doe', 'Mouse'),
(102, 'Jane Smith', 'Tablet'),
(102, 'Jane Smith', 'Keyboard'),
(102, 'Jane Smith', 'Mouse'),
(103, 'Emily Clark', 'Phone');

-- Question 2: Achieving 2NF (Second Normal Form)
-- The OrderDetails table has a partial dependency: CustomerName depends only on OrderID, not the full key (OrderID, Product).
-- To achieve 2NF, we split the table into two: one for order details and one for customer information.

-- Create Customers table to store CustomerName with OrderID
CREATE TABLE Customers (
    OrderID INT PRIMARY KEY,
    CustomerName VARCHAR(50)
);

-- Create OrderDetails_2NF table for product and quantity, linked to OrderID
CREATE TABLE OrderDetails_2NF (
    OrderID INT,
    Product VARCHAR(50),
    Quantity INT,
    PRIMARY KEY (OrderID, Product), -- Composite key
    FOREIGN KEY (OrderID) REFERENCES Customers(OrderID)
);

-- Insert data into Customers table
INSERT INTO Customers (OrderID, CustomerName) VALUES
(101, 'John Doe'),
(102, 'Jane Smith'),
(103, 'Emily Clark');

-- Insert data into OrderDetails_2NF table
INSERT INTO OrderDetails_2NF (OrderID, Product, Quantity) VALUES
(101, 'Laptop', 2),
(101, 'Mouse', 1),
(102, 'Tablet', 3),
(102, 'Keyboard', 1),
(102, 'Mouse', 2),
(103, 'Phone', 1);

 Step 1: Create Tables
CREATE TABLE Customers (
    CustomerID INT PRIMARY KEY,
    Name TEXT,
    City TEXT
);

CREATE TABLE Orders (
    OrderID INT PRIMARY KEY,
    CustomerID INT,
    Product TEXT,
    FOREIGN KEY (CustomerID) REFERENCES Customers(CustomerID)
);

 Step 2: Insert Sample Data
INSERT INTO Customers (CustomerID, Name, City) VALUES
(1, 'Alice', 'New York'),
(2, 'Bob', 'Los Angeles'),
(3, 'Charlie', 'Chicago'),
(4, 'Diana', 'Houston');

INSERT INTO Orders (OrderID, CustomerID, Product) VALUES
(101, 1, 'Laptop'),
(102, 2, 'Phone'),
(103, 1, 'Tablet'),
(104, 5, 'Camera'); -- Note: No customer with ID 5

 Step 3: Subqueries
  (SELECT COUNT(*) FROM Orders WHERE Orders.CustomerID = Customers.CustomerID) AS TotalOrders
FROM Customers;

 2. Subquery in WHERE using IN
SELECT * 
FROM Customers
WHERE CustomerID IN (SELECT CustomerID FROM Orders);

 3. Subquery in WHERE using EXISTS (correlated)
 List customers who have any orders
SELECT *
FROM Customers c
WHERE EXISTS (
    SELECT 1 FROM Orders o WHERE o.CustomerID = c.CustomerID
);

4. Subquery in FROM
 Show product count per customer using a subquery as a derived table
SELECT Name, OrderCount
FROM Customers
JOIN (
    SELECT CustomerID, COUNT(*) AS OrderCount
    FROM Orders
    GROUP BY CustomerID
) AS OrderSummary
ON Customers.CustomerID = OrderSummary.CustomerID;

 5. Subquery using '=' (scalar)
 Show customers who placed the **most number of orders**
SELECT Name
FROM Customers
WHERE CustomerID = (
    SELECT CustomerID 
    FROM Orders
    GROUP BY CustomerID
    ORDER BY COUNT(*) DESC
    LIMIT 1
);

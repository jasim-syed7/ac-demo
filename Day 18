### Detailed Explanation of Day 18: Subqueries – Advanced

**Objective**: Build on the foundational subquery knowledge from Day 17 to master advanced subquery techniques in Microsoft SQL Server (MSSQL). By the end of Day 18, you’ll be proficient in using subqueries in the `FROM` clause (derived tables), leveraging the `IN` and `EXISTS` operators, and handling multi-level nested subqueries. This day extends your Days 1-17 skills, particularly `SELECT`, joins, set operations, and basic subqueries, enabling you to tackle complex data retrieval scenarios like dynamic filtering, derived table joins, and hierarchical comparisons.

**Duration**: 5-7 hours (split into theory, hands-on practice, and review). Adjust based on your comfort with SQL Server Management Studio (SSMS) and SQL syntax.

---

### Sub-Syllabus Breakdown
1. **Subqueries in FROM (Derived Tables)** (1.5-2 hours)
   - Learn how to use subqueries in the `FROM` clause as derived tables.
   - Understand how to treat subquery results as temporary tables for further querying.
2. **IN, EXISTS Operators** (1.5-2 hours)
   - Master the `IN` and `EXISTS` operators for filtering based on subquery results.
   - Compare their behavior and performance in different scenarios.
3. **Multi-Level Nesting** (1.5-2 hours)
   - Explore subqueries nested within other subqueries for complex logic.
   - Apply these techniques in real-world scenarios like sales analysis or employee hierarchies.
4. **Practice with Real-World Scenarios** (1-1.5 hours)
   - Write 10-20 queries combining advanced subquery techniques.
   - Focus on practical applications like dynamic reports or conditional filtering.

---

### Step-by-Step Detailed Plan for Day 18

#### 1. Subqueries in FROM (Derived Tables) (1.5-2 hours)
**Goal**: Master the use of subqueries in the `FROM` clause to create derived tables, treating subquery results as temporary tables for further querying.

**Theory (45-60 minutes)**:
- **What are Derived Tables?**
  - A derived table is a subquery in the `FROM` clause that acts as a temporary table.
  - Must be aliased (e.g., `AS dt`) and enclosed in parentheses.
  - Allows you to perform additional operations (e.g., joins, aggregates) on the subquery’s results.
  - Example: Calculate department averages and join with employees:
    ```sql
    SELECT e.FirstName, e.Salary, dt.AvgSalary
    FROM HR.Employees AS e
    INNER JOIN (SELECT DepartmentID, AVG(Salary) AS AvgSalary
                FROM HR.Employees
                GROUP BY DepartmentID) AS dt
    ON e.DepartmentID = dt.DepartmentID;
    ```
    - The subquery `(SELECT ...)` creates a derived table `dt` with department averages.
- **Syntax**:
  - General form:
    ```sql
    SELECT Columns
    FROM Table1
    [JOIN] (SELECT Columns FROM Table2 WHERE Condition GROUP BY Columns) AS Alias
    ON Condition;
    ```
  - Key points:
    - Derived tables are temporary and exist only for the query’s duration.
    - Must have a unique alias (e.g., `dt`, `temp`).
    - Can include aggregates, filters, or joins within the subquery.
    - Useful when you need to preprocess data before joining or filtering.
- **Use Cases**:
  - Summarizing data before joining (e.g., department averages).
  - Simplifying complex logic by breaking it into steps.
  - Creating temporary result sets for further analysis (e.g., top customers by sales).
  - Example: Top orders by customer:
    ```sql
    SELECT c.CustomerName, dt.TotalSpent
    FROM Sales.Customers AS c
    INNER JOIN (SELECT CustomerID, SUM(TotalAmount) AS TotalSpent
                FROM Sales.Orders
                GROUP BY CustomerID) AS dt
    ON c.CustomerID = dt.CustomerID;
    ```
- **Key Points**:
  - Derived tables can replace correlated subqueries for better readability or performance.
  - Combine with `INNER JOIN`, `LEFT JOIN`, or other clauses.
  - Handle NULLs with `COALESCE` or `ISNULL` if needed.
  - Example with NULL handling:
    ```sql
    SELECT e.FirstName, COALESCE(dt.AvgSalary, 0) AS AvgSalary
    FROM HR.Employees AS e
    LEFT JOIN (SELECT DepartmentID, AVG(Salary) AS AvgSalary
               FROM HR.Employees
               GROUP BY DepartmentID) AS dt
    ON e.DepartmentID = dt.DepartmentID;
    ```
- **Best Practices**:
  - Use meaningful aliases (e.g., `DeptAvg` instead of `dt`).
  - Keep subqueries simple to improve readability.
  - Test the subquery independently to verify results.
  - Consider Common Table Expressions (CTEs, covered later) for reusable derived tables.
- **Common Pitfalls**:
  - Forgetting the alias for the derived table (causes syntax error).
  - Complex subqueries impacting performance (use indexes or optimize).
  - Ambiguous column names when joining derived tables.
- **SSMS Tools**:
  - View results: Results pane shows joined results with derived table data.
  - Debug errors: Messages pane shows “must specify table to select from” if alias is missing.
  - Execution plan: Right-click query > Display Estimated Execution Plan to assess performance.

**Practice (45-60 minutes)**:
- **Activity 1: Write Derived Table Queries**:
  - In your journal, write queries to:
    - Join employees with their department’s average salary.
    - List customers with their total order amounts.
    - Combine employee performance with department averages.
  - Example:
    ```sql
    SELECT e.FirstName, e.Salary, dt.AvgSalary
    FROM HR.Employees AS e
    INNER JOIN (SELECT DepartmentID, AVG(Salary) AS AvgSalary
                FROM HR.Employees
                GROUP BY DepartmentID) AS dt
    ON e.DepartmentID = dt.DepartmentID;
    ```
- **Activity 2: Run Derived Tables in SSMS**:
  - Use the `EmployeeDB` from Day 17:
    ```sql
    CREATE DATABASE EmployeeDB;
    USE EmployeeDB;
    CREATE SCHEMA HR;
    CREATE SCHEMA Sales;
    CREATE TABLE HR.Departments (
        DepartmentID INT PRIMARY KEY,
        DepartmentName VARCHAR(50) NOT NULL,
        Location VARCHAR(50)
    );
    INSERT INTO HR.Departments (DepartmentID, DepartmentName, Location)
    VALUES 
        (1, 'IT', 'New York'),
        (2, 'HR', 'Chicago'),
        (3, 'Finance', 'Boston'),
        (4, 'Marketing', 'San Francisco');
    CREATE TABLE HR.Employees (
        EmployeeID INT PRIMARY KEY,
        FirstName VARCHAR(50) NOT NULL,
        LastName VARCHAR(50) NOT NULL,
        Email VARCHAR(100) UNIQUE,
        DepartmentID INT,
        Salary DECIMAL(10,2),
        HireDate DATE DEFAULT GETDATE(),
        ManagerID INT,
        CONSTRAINT FK_Employee_Dept FOREIGN KEY (DepartmentID) REFERENCES HR.Departments(DepartmentID),
        CONSTRAINT FK_Employee_Manager FOREIGN KEY (ManagerID) REFERENCES HR.Employees(EmployeeID)
    );
    INSERT INTO HR.Employees (EmployeeID, FirstName, LastName, Email, DepartmentID, Salary, HireDate, ManagerID)
    VALUES 
        (1, 'Alice', 'Smith', 'alice.smith@company.com', 1, 75000.67, '2023-01-15', 3),
        (2, 'Bob', 'Johnson', 'bob.johnson@company.com', 2, 65000.33, '2023-02-20', 3),
        (3, 'Cathy', 'Lee', 'cathy.lee@company.com', 1, 80000.99, '2023-03-10', NULL),
        (4, 'David', 'Wong', 'david.wong@company.com', 3, 70000.45, '2023-04-05', NULL),
        (5, 'Emma', 'Brown', 'emma.brown@company.com', NULL, 60000.12, '2023-05-01', 4);
    CREATE TABLE HR.EmployeePerformance (
        PerformanceID INT PRIMARY KEY,
        EmployeeID INT,
        ReviewDate DATE,
        PerformanceScore INT,
        Comments VARCHAR(200),
        CONSTRAINT FK_Performance_Employee FOREIGN KEY (EmployeeID) REFERENCES HR.Employees(EmployeeID)
    );
    INSERT INTO HR.EmployeePerformance (PerformanceID, EmployeeID, ReviewDate, PerformanceScore, Comments)
    VALUES 
        (1, 1, '2025-01-15', 85, 'Excellent work'),
        (2, 2, '2025-02-20', 70, 'Needs improvement'),
        (3, 3, '2025-03-10', 90, 'Outstanding'),
        (4, 4, '2025-04-05', 65, 'Below expectations'),
        (5, 5, '2025-05-01', 80, 'Good performance');
    CREATE TABLE Sales.Customers (
        CustomerID INT PRIMARY KEY,
        CustomerName VARCHAR(50) NOT NULL,
        Email VARCHAR(100),
        Region VARCHAR(50)
    );
    INSERT INTO Sales.Customers (CustomerID, CustomerName, Email, Region)
    VALUES 
        (1, 'John Doe', 'john.doe@customer.com', 'East'),
        (2, 'Jane Smith', 'jane.smith@customer.com', 'West'),
        (3, 'Mike Brown', 'mike.brown@customer.com', 'North'),
        (4, 'Lisa Green', 'lisa.green@customer.com', 'San Francisco');
    CREATE TABLE Sales.Orders (
        OrderID INT PRIMARY KEY,
        CustomerID INT NOT NULL,
        OrderDate DATE NOT NULL,
        TotalAmount DECIMAL(10,2),
        Status VARCHAR(20),
        RepID INT,
        CONSTRAINT FK_Order_Customer FOREIGN KEY (CustomerID) REFERENCES Sales.Customers(CustomerID),
        CONSTRAINT FK_Order_Rep FOREIGN KEY (RepID) REFERENCES Sales.SalesReps(RepID)
    );
    INSERT INTO Sales.Orders (OrderID, CustomerID, OrderDate, TotalAmount, Status, RepID)
    VALUES 
        (1, 1, '2025-08-01', 199.99, 'Completed', 1),
        (2, 2, '2025-08-02', 49.99, 'Pending', 2),
        (3, 1, '2025-08-03', 299.99, 'Completed', 1),
        (4, 3, '2025-08-04', 99.99, 'Cancelled', 3),
        (5, 2, '2025-08-05', 149.99, 'Completed', 2);
    CREATE TABLE Sales.SalesReps (
        RepID INT PRIMARY KEY,
        EmployeeID INT,
        Region VARCHAR(50),
        CONSTRAINT FK_Rep_Employee FOREIGN KEY (EmployeeID) REFERENCES HR.Employees(EmployeeID)
    );
    INSERT INTO Sales.SalesReps (RepID, EmployeeID, Region)
    VALUES 
        (1, 1, 'East'),
        (2, 3, 'West'),
        (3, 4, 'North');
    ```
  - Run:
    ```sql
    -- Department average salary
    SELECT e.FirstName, e.Salary, dt.AvgSalary
    FROM HR.Employees AS e
    INNER JOIN (SELECT DepartmentID, AVG(Salary) AS AvgSalary
                FROM HR.Employees
                GROUP BY DepartmentID) AS dt
    ON e.DepartmentID = dt.DepartmentID;
    -- Customer total orders
    SELECT c.CustomerName, dt.TotalSpent
    FROM Sales.Customers AS c
    INNER JOIN (SELECT CustomerID, SUM(TotalAmount) AS TotalSpent
                FROM Sales.Orders
                GROUP BY CustomerID) AS dt
    ON c.CustomerID = dt.CustomerID;
    ```
  - Verify: Check Results pane for joined results (e.g., Alice’s salary with IT average).
- **Activity 3: Test with Joins and Functions**:
  - Run:
    ```sql
    SELECT e.FirstName, p.PerformanceScore, dt.AvgScore
    FROM HR.Employees AS e
    INNER JOIN HR.EmployeePerformance AS p ON e.EmployeeID = p.EmployeeID
    LEFT JOIN (SELECT EmployeeID, AVG(PerformanceScore) AS AvgScore
               FROM HR.EmployeePerformance
               GROUP BY EmployeeID) AS dt
    ON e.EmployeeID = dt.EmployeeID;
    ```
- **Activity 4: Test Errors**:
  - Try: `SELECT e.FirstName FROM HR.Employees AS e JOIN (SELECT DepartmentID, AVG(Salary) FROM HR.Employees GROUP BY DepartmentID) ON e.DepartmentID = DepartmentID;` (fails; missing alias).
  - Fix: Add `AS dt` and use `dt.DepartmentID`.

**Resources**:
- Microsoft Docs: “Subqueries (Transact-SQL)” (search “SQL Server derived tables”).
- W3Schools: SQL Subqueries tutorial.
- SQLZoo: Interactive derived table exercises.

---

#### 2. IN, EXISTS Operators (1.5-2 hours)
**Goal**: Master the `IN` and `EXISTS` operators for filtering rows based on subquery results, understanding their differences and performance.

**Theory (30-45 minutes)**:
- **IN Operator**:
  - Tests if a value is in a list returned by a subquery (multiple rows, one column).
  - Syntax:
    ```sql
    SELECT Columns
    FROM Table1
    WHERE Column IN (SELECT Column FROM Table2 WHERE Condition);
    ```
  - Example:
    ```sql
    SELECT FirstName
    FROM HR.Employees
    WHERE DepartmentID IN (SELECT DepartmentID FROM HR.Departments WHERE Location = 'New York');
    ```
    - Returns employees in departments located in New York.
  - Key points:
    - Subquery must return one column.
    - Can handle NULLs (use `NOT IN` cautiously, as NULLs can cause unexpected results).
    - Alternative to multiple `OR` conditions.
- **EXISTS Operator**:
  - Tests if a subquery returns any rows (correlated subquery).
  - Syntax:
    ```sql
    SELECT Columns
    FROM Table1 AS t1
    WHERE EXISTS (SELECT 1 FROM Table2 AS t2 WHERE t2.Column = t1.Column AND Condition);
    ```
  - Example:
    ```sql
    SELECT c.CustomerName
    FROM Sales.Customers AS c
    WHERE EXISTS (SELECT 1 FROM Sales.Orders AS o WHERE o.CustomerID = c.CustomerID AND o.Status = 'Completed');
    ```
    - Returns customers with completed orders.
  - Key points:
    - Stops processing once a row is found (potentially faster than `IN`).
    - Always correlated; uses outer query values.
    - Returns `TRUE` or `FALSE`, not data.
- **Comparison**:
  - **IN**:
    - Non-correlated or correlated.
    - Returns a list of values for comparison.
    - Slower for large datasets (processes entire list).
    - Example: `WHERE DepartmentID IN (SELECT DepartmentID FROM HR.Departments)`.
  - **EXISTS**:
    - Always correlated.
    - Checks for existence, stops at first match.
    - Often faster for large datasets or indexed tables.
    - Example: `WHERE EXISTS (SELECT 1 FROM Sales.Orders WHERE CustomerID = c.CustomerID)`.
- **Performance Considerations**:
  - `IN` retrieves all subquery results before filtering, which can be slow for large lists.
  - `EXISTS` stops at the first match, leveraging indexes effectively.
  - Use `IN` for small, non-correlated subqueries; use `EXISTS` for correlated checks.
  - Joins may outperform both in some cases (e.g., `INNER JOIN` vs. `EXISTS`).
- **Best Practices**:
  - Use `IN` for small, static lists or non-correlated subqueries.
  - Use `EXISTS` for correlated checks or large datasets.
  - Avoid `NOT IN` with NULLs; use `NOT EXISTS` instead.
  - Test subqueries independently to verify results.
- **Common Pitfalls**:
  - `NOT IN` failing due to NULLs in subquery (e.g., no rows returned).
  - Overusing correlated subqueries when joins are simpler.
  - Subquery returning multiple columns (causes error).
- **SSMS Tools**:
  - Execution plan: Compare `IN` vs. `EXISTS` performance.
  - Debug errors: Messages pane shows “Subquery returned more than 1 column” or NULL issues.

**Practice (45-75 minutes)**:
- **Activity 1: Write IN and EXISTS Queries**:
  - In your journal, write queries to:
    - Find employees in specific departments using `IN`.
    - List customers with orders using `EXISTS`.
    - Identify employees without performance reviews using `NOT EXISTS`.
  - Example:
    ```sql
    -- IN
    SELECT FirstName
    FROM HR.Employees
    WHERE DepartmentID IN (SELECT DepartmentID FROM HR.Departments WHERE Location IN ('New York', 'Chicago'));
    -- EXISTS
    SELECT CustomerName
    FROM Sales.Customers AS c
    WHERE EXISTS (SELECT 1 FROM Sales.Orders AS o WHERE o.CustomerID = c.CustomerID);
    ```
- **Activity 2: Run in SSMS**:
  - Run:
    ```sql
    -- IN: Employees in high-salary departments
    SELECT FirstName, Salary
    FROM HR.Employees
    WHERE DepartmentID IN (SELECT DepartmentID FROM HR.Employees GROUP BY DepartmentID HAVING AVG(Salary) > 70000);
    -- EXISTS: Customers with high-value orders
    SELECT CustomerName
    FROM Sales.Customers AS c
    WHERE EXISTS (SELECT 1 FROM Sales.Orders AS o WHERE o.CustomerID = c.CustomerID AND o.TotalAmount > 100);
    -- NOT EXISTS: Employees without orders
    SELECT e.FirstName
    FROM HR.Employees AS e
    WHERE NOT EXISTS (SELECT 1 FROM Sales.SalesReps AS r WHERE r.EmployeeID = e.EmployeeID);
    ```
  - Verify: Check Results pane for correct filtering.
- **Activity 3: Test Errors**:
  - Try: `SELECT FirstName FROM HR.Employees WHERE DepartmentID IN (SELECT DepartmentID, DepartmentName FROM HR.Departments);` (fails; multiple columns).
  - Fix: Use `SELECT DepartmentID`.

---

#### 3. Multi-Level Nesting (1.5-2 hours)
**Goal**: Master multi-level nested subqueries to solve complex problems, combining derived tables, `IN`, and `EXISTS`.

**Theory (30-45 minutes)**:
- **What is Multi-Level Nesting?**
  - Subqueries nested within other subqueries, often two or more levels deep.
  - Used for complex filtering or calculations (e.g., employees in departments with high average salaries).
  - Example:
    ```sql
    SELECT FirstName
    FROM HR.Employees
    WHERE DepartmentID IN (SELECT DepartmentID
                           FROM HR.Employees
                           WHERE Salary > (SELECT AVG(Salary) FROM HR.Employees));
    ```
    - Inner subquery finds average salary; middle subquery finds departments with high salaries; outer query finds employees.
- **Syntax**:
  - General form:
    ```sql
    SELECT Columns
    FROM Table1
    WHERE Column IN/EXISTS (SELECT Column
                            FROM Table2
                            WHERE Condition IN/EXISTS (SELECT Column FROM Table3 WHERE Condition));
    ```
  - Key points:
    - Each subquery must return appropriate rows/columns (scalar for `=`, multi-row for `IN`).
    - Nesting increases complexity; consider CTEs or joins for readability.
    - Example with derived table:
      ```sql
      SELECT e.FirstName, dt.TotalSpent
      FROM HR.Employees AS e
      INNER JOIN (SELECT r.EmployeeID, SUM(o.TotalAmount) AS TotalSpent
                  FROM Sales.SalesReps AS r
                  INNER JOIN Sales.Orders AS o ON r.RepID = o.RepID
                  WHERE o.CustomerID IN (SELECT CustomerID FROM Sales.Customers WHERE Region = 'East')
                  GROUP BY r.EmployeeID) AS dt
      ON e.EmployeeID = dt.EmployeeID;
      ```
- **Use Cases**:
  - Hierarchical filtering (e.g., employees in high-performing departments).
  - Multi-step calculations (e.g., top customers by region and order volume).
  - Complex business logic (e.g., sales reps with above-average performance in specific regions).
- **Best Practices**:
  - Break down nested queries into steps for testing.
  - Use comments to document each subquery’s purpose.
  - Consider performance; deep nesting can be slow (use Execution Plan).
  - Replace with joins or CTEs if nesting becomes unwieldy.
- **Common Pitfalls**:
  - Subqueries returning unexpected rows/columns.
  - Performance degradation with deep nesting or large datasets.
  - Difficult debugging due to complexity.
- **SSMS Tools**:
  - Test subqueries: Run inner queries independently.
  - Execution plan: Analyze nested query performance.
  - Debug errors: Check Messages pane for row/column issues.

**Practice (45-75 minutes)**:
- **Activity 1: Write Nested Queries**:
  - In your journal, write queries to:
    - Find employees in departments with above-average salaries.
    - List customers with orders in high-value regions.
    - Identify sales reps with top performance scores.
  - Example:
    ```sql
    SELECT FirstName
    FROM HR.Employees
    WHERE DepartmentID IN (SELECT DepartmentID
                           FROM HR.Employees
                           WHERE Salary > (SELECT AVG(Salary) FROM HR.Employees));
    ```
- **Activity 2: Run in SSMS**:
  - Run:
    ```sql
    -- Nested IN: Employees in high-salary departments
    SELECT FirstName, Salary
    FROM HR.Employees
    WHERE DepartmentID IN (SELECT DepartmentID
                           FROM HR.Employees
                           GROUP BY DepartmentID
                           HAVING AVG(Salary) > (SELECT AVG(Salary) FROM HR.Employees));
    -- Nested EXISTS: Customers with high-value orders
    SELECT CustomerName
    FROM Sales.Customers AS c
    WHERE EXISTS (SELECT 1
                  FROM Sales.Orders AS o
                  WHERE o.CustomerID = c.CustomerID
                  AND o.TotalAmount > (SELECT AVG(TotalAmount) FROM Sales.Orders));
    -- Derived table with nesting
    SELECT e.FirstName, dt.TotalSales
    FROM HR.Employees AS e
    INNER JOIN (SELECT r.EmployeeID, SUM(o.TotalAmount) AS TotalSales
                FROM Sales.SalesReps AS r
                INNER JOIN Sales.Orders AS o ON r.RepID = o.RepID
                WHERE o.CustomerID I

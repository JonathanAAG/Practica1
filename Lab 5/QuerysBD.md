# Task 

## 1. SQL Query Optimization

### Orders Query
```
    SELECT Orders.OrderID, SUM(OrderDetails.Quantity * OrderDetails.UnitPrice) AS TotalPrice
    FROM Orders
    JOIN OrderDetails ON Orders.OrderID = OrderDetails.OrderID
    WHERE OrderDetails.Quantity > 10
    GROUP BY Orders.OrderID;
```

### Resultado
Se podrian agregar indices para mejorar la eficiencia de la consulta

```
CREATE INDEX idx_orderdetails_quantity ON OrderDetails(Quantity);
```

### Customer Query
```
    SELECT CustomerName FROM Customers WHERE City = 'London' ORDER BY CustomerName;
```
### Resultado
Se podrian agregar indices para mejorar la eficiencia de la consulta

```
CREATE INDEX idx_customer_city ON Customers (City);
```
## 2. NoSQL Query Implementation

### User Posts Query
```
    db.posts
    .find({ status: "active" }, { title: 1, likes: 1 })
    .sort({ likes: -1 });
```

### Resultado
Se podrian agregar indices compuesto para mejorar la eficiencia de la consulta
```
db.posts.createIndex({ status: 1, likes: -1 });
```

### User Data Aggregation
```
    db.users.aggregate([
    { $match: { status: "active" } },
    { $group: { _id: "$location", totalUsers: { $sum: 1 } } },
    ]);
```

### Resultado
Se podrian agregar indices compuesto para mejorar la eficiencia de la consulta
```
db.users.createIndex({ status: 1 });
```
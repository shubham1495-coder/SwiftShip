# SwiftShip

A comprehensive database solution for managing shipments, delivery partners, and logistics operations with powerful analytics and reporting capabilities.

## Overview

SwiftShip is a SQL-based logistics management system designed to track shipments, manage delivery partners, and monitor delivery performance. The system provides detailed insights into delivery statuses, partner performance metrics, and identifies areas for operational improvement.

## Project Structure

The repository contains:

- **Schema** - Database structure and sample data initialization
  - Partners table - Stores delivery partner information
  - Shipments table - Tracks individual shipments and their details
  - DeliveryLogs table - Records delivery outcomes and statuses

- **Query** - Pre-built SQL queries for analytics and reporting
  - Partner performance analysis
  - Destination popularity metrics
  - Delivery delay identification
  - Partner scorecard generation

## Database Schema

### Partners Table
```sql
CREATE TABLE Partners (
    PartnerID INT PRIMARY KEY,
    PartnerName VARCHAR(100)
);
```

### Shipments Table
```sql
CREATE TABLE Shipments (
    ShipmentID INT PRIMARY KEY,
    PartnerID INT,
    OrderDate DATE,
    PromisedDate DATE,
    DestinationCity VARCHAR(100),
    FOREIGN KEY (PartnerID) REFERENCES Partners(PartnerID)
);
```

### DeliveryLogs Table
```sql
CREATE TABLE DeliveryLogs (
    LogID INT PRIMARY KEY,
    ShipmentID INT,
    ActualDeliveryDate DATE,
    DeliveryStatus VARCHAR(50),
    FOREIGN KEY (ShipmentID) REFERENCES Shipments(ShipmentID)
);
```

## Key Features

### 1. Partner Performance Analysis
Tracks successful vs. returned deliveries for each partner, enabling performance comparison.

### 2. Destination Analytics
Identifies the most popular destination cities based on order volume in specified time periods.

### 3. Delay Detection
Identifies shipments where actual delivery dates exceed promised dates, highlighting performance issues.

### 4. Partner Scorecard
Ranks partners by fewest delays, providing a clear performance hierarchy for quality assurance.

## Sample Data

The database comes pre-loaded with sample data:

- **3 Partners**: Alpha Express, Beta Courier, Gamma Freight
- **6 Shipments**: Distributed across multiple cities and date ranges
- **6 Delivery Logs**: Various delivery statuses (Successful, Returned)

## Usage Examples

### Get Partner Performance
```sql
SELECT 
    p.PartnerName, 
    d.DeliveryStatus, 
    COUNT(d.LogID) AS TotalDeliveries
FROM Partners p
JOIN Shipments s ON p.PartnerID = s.PartnerID
JOIN DeliveryLogs d ON s.ShipmentID = d.ShipmentID
WHERE d.DeliveryStatus IN ('Successful', 'Returned')
GROUP BY p.PartnerName, d.DeliveryStatus
ORDER BY p.PartnerName, d.DeliveryStatus;
```

### Find Most Popular Destination
```sql
SELECT 
    DestinationCity, 
    COUNT(ShipmentID) AS OrderCount
FROM Shipments
WHERE OrderDate >= CURRENT_DATE - INTERVAL 30 DAY
GROUP BY DestinationCity
ORDER BY OrderCount DESC
LIMIT 1;
```

### Identify Late Deliveries
```sql
SELECT 
    s.ShipmentID, 
    p.PartnerName, 
    s.PromisedDate, 
    d.ActualDeliveryDate
FROM Shipments s
JOIN DeliveryLogs d ON s.ShipmentID = d.ShipmentID
JOIN Partners p ON s.PartnerID = p.PartnerID
WHERE d.ActualDeliveryDate > s.PromisedDate;
```

## Installation

1. Clone the repository
2. Execute the Schema file to create the database and tables
3. Use the Query file for analytics and reporting

## Technologies

- **Database**: MySQL/SQL
- **Query Language**: SQL
- **Focus**: Logistics and Delivery Management

## Use Cases

- Track shipment status across multiple delivery partners
- Monitor delivery performance and identify bottlenecks
- Analyze destination popularity for demand planning
- Evaluate partner reliability based on delivery metrics
- Generate performance scorecards for partner management

## Future Enhancements

- Real-time tracking capabilities
- Integration with shipping APIs
- Machine learning-based delay prediction
- Dashboard visualization
- Customer notification system

## License

Open source project for logistics management

## Author

Created by shubham1495-coder
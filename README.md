# 📊 Sales & Returns Analysis – Power BI Data Modeling Project

## 📌 Project Overview
This project demonstrates **end-to-end data modeling and analysis in Power BI** using a **Star Schema** and **Fact Constellation (Galaxy Schema)** approach.  
The objective is to validate relationships, schema design, and analytical correctness using **only Matrix visuals**.

The dataset represents **Sales and Returns transactions** across customers, products, regions, and time.

---

## 🗂️ Datasets Used

### 1️⃣ Sales_Fact.xlsx
| Column | Description |
|------|------------|
SalesID (PK) | Unique sales transaction ID  
CustomerID (FK) | Customer reference  
ProductID (FK) | Product reference  
RegionID (FK) | Region reference  
DateKey (FK) | Sales date key (YYYYMMDD)  
Quantity | Units sold  
Revenue | Sales revenue  
Discount | Discount applied  

---

### 2️⃣ Customer_Dim.xlsx
| Column | Description |
|------|------------|
CustomerID (PK) | Customer identifier  
FullName | Customer name  
Age | Customer age  
Gender | Gender  
Segment | Customer segment  

---

### 3️⃣ Product_Dim.xlsx
| Column | Description |
|------|------------|
ProductID (PK) | Product identifier  
ProductName | Product name  
Category | Product category  
Subcategory | Product subcategory  
Brand | Brand name  

---

### 4️⃣ Region_Dim.xlsx
| Column | Description |
|------|------------|
RegionID (PK) | Region identifier  
Country | Country  
State | State  
City | City  

---

### 5️⃣ Date_Dim.xlsx
| Column | Description |
|------|------------|
DateKey (PK) | Date key (YYYYMMDD)  
Date | Calendar date  
Month | Month name  
Quarter | Quarter  
Year | Calendar year  
Fiscal Year | Fiscal year  

---

### 6️⃣ Returns_Fact.xlsx
| Column | Description |
|------|------------|
ReturnID (PK) | Return transaction ID  
SalesID (FK) | Linked sales transaction  
ReturnDateKey (FK) | Return date key  
Reason | Reason for return  

---

## 🧠 Data Modeling & Schema Design

### ⭐ Star Schema (Primary)
- **Sales_Fact** is the central fact table
- Connected directly to:
  - Customer_Dim
  - Product_Dim
  - Region_Dim
  - Date_Dim

This design ensures:
- Optimal performance
- Simple DAX calculations
- Correct filter propagation

---

### 🔶 Fact Constellation (Galaxy Schema)
- **Returns_Fact** modeled as a **second fact table**
- Linked to:
  - Sales_Fact (SalesID)
  - Date_Dim (ReturnDateKey – inactive relationship)

This reflects a **separate business process (Returns)** with a different grain.

---

## 🔗 Relationships Created

| From Table | To Table | Key | Cardinality | Status |
|-----------|---------|----|-------------|-------|
Sales_Fact | Customer_Dim | CustomerID | Many → One | Active |
Sales_Fact | Product_Dim | ProductID | Many → One | Active |
Sales_Fact | Region_Dim | RegionID | Many → One | Active |
Sales_Fact | Date_Dim | DateKey | Many → One | Active |
Returns_Fact | Sales_Fact | SalesID | Many → One | Active |
Returns_Fact | Date_Dim | ReturnDateKey | Many → One | Inactive |

---

## 📐 Cardinality Explanation
- **Many → One**: Multiple fact rows map to a single dimension row  
- Used across all fact-to-dimension relationships  
- Ensures no data duplication and correct aggregation  

---

## 📊 Measures Created (DAX)

```DAX
Total Revenue :=
SUM ( Sales_Fact[Revenue] )

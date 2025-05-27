# Fleet Management Database - Schema Documentation

## Database Schema Analysis

This document contains the complete table structure analysis for the Fleet Management System database.

---

## Database Design Analysis

### Key Relationships & Design Questions

**EMPLOYEE vs DRIVER Tables:**
- **EMPLOYEE**: Contains basic staff information (all company employees - drivers, dispatchers, mechanics, etc.)
- **DRIVER**: Contains only driving-specific certifications and licenses (subset of employees)
- **Relationship**: DRIVER.EMP_NUM → EMPLOYEE.EMP_NUM (not all employees are drivers)

### Primary Keys (PK)
- VEHICLE.VEH_NUMBER
- TRIP.TRIP_ID  
- CUSTOMER.CUS_CODE
- EMPLOYEE.EMP_NUM
- MODEL.MOD_CODE
- DRIVER.EMP_NUM (also FK to EMPLOYEE)

### Foreign Keys (FK)
- VEHICLE.MOD_CODE → MODEL.MOD_CODE
- TRIP.TRIP_DRIVER → EMPLOYEE.EMP_NUM
- TRIP.VEH_NUMBER → VEHICLE.VEH_NUMBER
- TRIP.CUS_CODE → CUSTOMER.CUS_CODE
- DRIVER.EMP_NUM → EMPLOYEE.EMP_NUM

---

## Table Structures

### VEHICLE Table - Fleet inventory and specs
```
+-----------+-------------+------+-----+---------+-------+
| Field     | Type        | Null | Key | Default | Extra |
+-----------+-------------+------+-----+---------+-------+
| VEH_NUMBER| varchar(5)  | NO   | PK  | NULL    |       |
| MOD_CODE  | varchar(10) | NO   | FK  | NULL    |       |
| VEH_TTAF  | float       | YES  |     | NULL    |       |
| VEH_TTEL  | float       | YES  |     | NULL    |       |
| VEH_TTER  | float       | YES  |     | NULL    |       |
+-----------+-------------+------+-----+---------+-------+
```

### TRIP Table - Trip records and details
```
+------------------+------------+------+-----+---------+-------+
| Field            | Type       | Null | Key | Default | Extra |
+------------------+------------+------+-----+---------+-------+
| TRIP_ID          | int(11)    | NO   | PK  | NULL    |       |
| TRIP_DATE        | datetime   | YES  |     | NULL    |       |
| TRIP_DRIVER      | int(11)    | NO   | FK  | NULL    |       |
| VEH_NUMBER       | varchar(5) | NO   | FK  | NULL    |       |
| TRIP_DESTINATION | varchar(3) | YES  |     | NULL    |       |
| TRIP_DISTANCE    | float      | YES  |     | NULL    |       |
| TRIP_HOURS       | float      | YES  |     | NULL    |       |
| TRIP_WAIT_TIME   | float      | YES  |     | NULL    |       |
| TRIP_FUEL_USED   | float      | YES  |     | NULL    |       |
| TRIP_MAINTENANCE | int(11)    | YES  |     | NULL    |       |
| CUS_CODE         | int(11)    | NO   | FK  | NULL    |       |
+------------------+------------+------+-----+---------+-------+
```

### CUSTOMER Table - Customer accounts
```
+--------------+---------------+------+-----+---------+-------+
| Field        | Type          | Null | Key | Default | Extra |
+--------------+---------------+------+-----+---------+-------+
| CUS_CODE     | int(11)       | NO   | PK  | NULL    |       |
| CUS_LNAME    | varchar(15)   | YES  |     | NULL    |       |
| CUS_FNAME    | varchar(15)   | YES  |     | NULL    |       |
| CUS_INITIAL  | varchar(1)    | YES  |     | NULL    |       |
| CUS_AREACODE | varchar(3)    | YES  |     | NULL    |       |
| CUS_PHONE    | varchar(8)    | YES  |     | NULL    |       |
| CUS_BALANCE  | decimal(10,0) | YES  |     | NULL    |       |
+--------------+---------------+------+-----+---------+-------+
```

### EMPLOYEE Table - Staff information
```
+---------------+-------------+------+-----+---------+-------+
| Field         | Type        | Null | Key | Default | Extra |
+---------------+-------------+------+-----+---------+-------+
| EMP_NUM       | int(11)     | NO   | PK  | NULL    |       |
| EMP_TITLE     | varchar(4)  | YES  |     | NULL    |       |
| EMP_LNAME     | varchar(15) | YES  |     | NULL    |       |
| EMP_FNAME     | varchar(15) | YES  |     | NULL    |       |
| EMP_INITIAL   | varchar(1)  | YES  |     | NULL    |       |
| EMP_DOB       | datetime    | YES  |     | NULL    |       |
| EMP_HIRE_DATE | datetime    | YES  |     | NULL    |       |
+---------------+-------------+------+-----+---------+-------+
```

### MODEL Table - Vehicle specifications
```
+------------------+---------------+------+-----+---------+-------+
| Field            | Type          | Null | Key | Default | Extra |
+------------------+---------------+------+-----+---------+-------+
| MOD_CODE         | varchar(10)   | NO   | PK  | NULL    |       |
| MOD_MANUFACTURER | varchar(15)   | YES  |     | NULL    |       |
| MOD_NAME         | varchar(20)   | YES  |     | NULL    |       |
| MOD_SEATS        | float         | YES  |     | NULL    |       |
| MOD_RATE_MILE    | decimal(10,0) | YES  |     | NULL    |       |
+------------------+---------------+------+-----+---------+-------+
```

### DRIVER Table - Certifications and licenses
```
+----------------+-------------+------+-----+---------+-------+
| Field          | Type        | Null | Key | Default | Extra |
+----------------+-------------+------+-----+---------+-------+
| EMP_NUM        | int(11)     | NO   | PK/FK| NULL   |       |
| DRV_LICENSE    | varchar(25) | YES  |     | NULL    |       |
| DRV_RATINGS    | varchar(30) | YES  |     | NULL    |       |
| DRV_MED_TYPE   | varchar(1)  | YES  |     | NULL    |       |
| DRV_MED_DATE   | datetime    | YES  |     | NULL    |       |
| DRV_CERT_DATE  | datetime    | YES  |     | NULL    |       |
+----------------+-------------+------+-----+---------+-------+
```

---

*Generated from MySQL DESCRIBE commands for Fleet Management System*
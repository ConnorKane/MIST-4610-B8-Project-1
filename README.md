# MIST-4610-B8-Project-1

# Section 1
### Group B8: 
Connor Kane, Ava Garrett, Kundan Vadlamani, Ben McDonald


# Section 2
### Case Description: 
The Campus Club Framework (CCF) is a comprehensive system designed to manage and support student organizations, their members, and related activities within a university. It maintains detailed records of clubs, including their identity, contact information, and classification, while also tracking student participation through memberships and leadership roles. The system supports event planning and coordination, including co-hosted events and attendance tracking, ensuring accurate records of participation and service hours. Additionally, CCF manages operational aspects such as club fair participation, budgeting by academic term, and funding workflows from request submission and staff review to purchases and reimbursements. While these components are interconnected, they remain flexible and independent, allowing clubs, events, and student involvement to function even without direct dependencies on funding, membership, or other records.

### Extension: 
Our extension is a staff entity, which allows us to store and acsess details about the staff members who interact with the CCF through the budget review process.


# Section 3 Data Model

<img width="1858" height="1055" alt="modelPic" src="https://github.com/user-attachments/assets/b2dfcf16-1cda-4c32-836a-61b69d22147b" />

### Data model explanation

Our model represents a fictitious structure of a Campus Club Fair at Peachtree State University. In this database, student organizations, memberships, events, attendance, and club funding can all be managed and tracked with ease. A student can join many clubs, and each of these respective clubs can have many students. This many to many relationship is expressed through the membership table. The membership table records a student’s status after they join a club. These students can then adopt leadership positions in these clubs. This is then recorded in the officer role entity which links a certain student to a specific club and shows the role name the student is associated with and the time period of their leadership. This allows clubs to track leadership changes over a certain time. 

In our model, we have illustrated the multitude of relationships that relate to the club entity. A club can be associated with multiple events. Events themselves can be hosted by many clubs which then results in another many to many relationship which is joined by the event host table. Event participation is tracked through the event attendance entity. This table connects students to events and records their check-in status and service hours. Clubs can then participate in club fairs. This relationship is then managed through the fair reservation table. This entity assigns clubs to specific fairs and records details like table numbers and access requests.

Equipment and finances are also captured through our model. Equipment is owned by clubs, and students can borrow equipment through the equipment checkout relationship. This relationship connects students, equipment, and equipment condition to track equipment usage and returns. Each club is associated with an assigned budget, and clubs can submit funding requests tied to specific events. Staff then reviews these requests, and once they are approved, a purchase can then be made. These purchases are directly linked back to the original funding request, an event, and the club associated with the event. These relationships ensure that entities associated with club operations are organized and traceable. 





# Section 4 Data Dictionary

## BUDGET
| Column Name     | Data Type     | Key | Description                          |
|----------------|--------------|-----|--------------------------------------|
| BudgetID       | int          | PK  | Unique identifier for each budget    |
| Term           | varchar(45)  |     | Academic term associated with budget |
| BudgetAmount   | decimal(45,0)|     | Allocated budget amount              |
| BudgetDateSet  | date         |     | Date the budget was established      |
| ClubID         | int          | FK  | References CLUB(ClubID)              |

## CLUB
| Column Name        | Data Type    | Key | Description                              |
|--------------------|-------------|-----|------------------------------------------|
| ClubID            | int         | PK  | Unique identifier for each club          |
| ClubName          | varchar(45) |     | Name of the club                         |
| Category          | varchar(45) |     | Category or type of club                 |
| DateEstablished   | date        |     | Date the club was established            |
| PrimaryEmail      | varchar(45) |     | Primary contact email                    |
| WebsiteSocialLink | varchar(45) |     | Website or social media link             |
| SisterClubID      | int         | FK  | References CLUB(ClubID)                  |

## CLUB_FAIR
| Column Name | Data Type    | Key | Description                     |
|------------|-------------|-----|---------------------------------|
| FairID     | int         | PK  | Unique identifier for club fair |
| Date       | date        |     | Date of the fair                |
| StartTime  | time        |     | Start time                      |
| EndTime    | time        |     | End time                        |
| VenueName  | varchar(45) |     | Location of the fair            |

## EQUIPMENT
| Column Name   | Data Type     | Key | Description                     |
|--------------|--------------|-----|---------------------------------|
| EquipmentID  | int          | PK  | Unique identifier for equipment |
| ItemName     | varchar(45)  |     | Name of the item                |
| Description  | varchar(45)  |     | Description of the item         |
| PurchaseValue| decimal(45,0)|     | Purchase cost                   |
| ClubID       | int          | FK  | References CLUB(ClubID)         |

## CHECKOUT
| Column Name        | Data Type | Key    | Description                          |
|--------------------|----------|--------|--------------------------------------|
| CheckoutID        | int      | PK     | Unique checkout identifier           |
| CheckoutDate      | date     |        | Date item was checked out            |
| ReturnDueDate     | date     |        | Expected return date                 |
| ActualReturnDate  | date     |        | Actual return date                   |
| StudentID         | int      | FK     | References STUDENT(StudentID)        |
| EquipmentID       | int      | PK, FK | References EQUIPMENT(EquipmentID)    |
| ConditionID_Out   | int      | FK     | Condition at checkout                |
| ConditionID_Return| int      | FK     | Condition at return                  |

## EQUIPMENT_CONDITION
| Column Name   | Data Type    | Key | Description                  |
|--------------|-------------|-----|------------------------------|
| ConditionID  | int         | PK  | Unique condition identifier  |
| ConditionName| varchar(45) |     | Description of condition     |

## EVENT
| Column Name | Data Type    | Key | Description             |
|------------|-------------|-----|-------------------------|
| EventID    | int         | PK  | Unique event identifier |
| EventTitle | varchar(45) |     | Title of the event      |
| Date       | date        |     | Event date              |
| StartTime  | time        |     | Start time              |
| EndTime    | time        |     | End time                |
| Location   | varchar(45) |     | Event location          |

## ATTENDANCE
| Column Name        | Data Type | Key    | Description                      |
|--------------------|----------|--------|----------------------------------|
| AttendanceID      | int      | PK     | Unique attendance record         |
| CheckedIn         | tinyint  |        | Check-in status                  |
| EarnedServiceHours| tinyint  |        | Service hours indicator          |
| NumberOfHours     | int      |        | Number of hours earned           |
| StudentID         | int      | PK, FK | References STUDENT(StudentID)    |
| EventID           | int      | PK, FK | References EVENT(EventID)        |

## HOST
| Column Name | Data Type    | Key | Description               |
|------------|-------------|-----|---------------------------|
| HostID     | int         | PK  | Unique host identifier    |
| HostType   | varchar(45) |     | Type of host              |
| EventID    | int         | FK  | References EVENT(EventID) |
| ClubID     | int         | FK  | References CLUB(ClubID)   |

## RESERVATION
| Column Name     | Data Type | Key    | Description                    |
|----------------|----------|--------|--------------------------------|
| ReservationID  | int      | PK     | Unique reservation identifier  |
| TableNumber    | int      |        | Assigned table number          |
| RequestedPower | tinyint  |        | Power access required          |
| FairID         | int      | PK, FK | References CLUB_FAIR(FairID)   |
| ClubID         | int      | PK, FK | References CLUB(ClubID)        |

## REQUEST
| Column Name        | Data Type     | Key | Description                    |
|--------------------|--------------|-----|--------------------------------|
| RequestID         | int          | PK  | Unique request identifier      |
| RequestDate       | date         |     | Date submitted                 |
| RequestedAmount   | decimal(45,0)|     | Amount requested               |
| PurposeDescription| varchar(45)  |     | Purpose of request             |
| Status            | varchar(45)  |     | Approval status                |
| ApprovedAmount    | decimal(45,0)|     | Amount approved                |
| DecisionDate      | date         |     | Decision date                  |
| EventID           | int          | FK  | References EVENT(EventID)      |
| ClubID            | int          | FK  | References CLUB(ClubID)        |
| StaffID_Approver  | int          | FK  | Primary approver               |
| StaffID_Reviewer  | int          | FK  | Secondary reviewer             |

## MEMBERSHIP
| Column Name       | Data Type    | Key    | Description                   |
|------------------|-------------|--------|-------------------------------|
| MembershipID     | int         | PK     | Unique membership identifier  |
| ClubID           | int         | FK     | References CLUB(ClubID)       |
| JoinDate         | date        |        | Date joined                   |
| MembershipStatus | varchar(45) |        | Status                        |
| StudentID        | int         | PK, FK | References STUDENT(StudentID) |

## ROLE
| Column Name | Data Type    | Key | Description               |
|------------|-------------|-----|---------------------------|
| RoleID     | int         | PK  | Unique role identifier    |
| RoleName   | varchar(45) |     | Role name                 |
| StartDate  | date        |     | Start date                |
| EndDate    | date        |     | End date                  |
| StudentID  | int         | FK  | References STUDENT        |
| ClubID     | int         | FK  | References CLUB           |

## PURCHASE
| Column Name | Data Type     | Key | Description               |
|------------|--------------|-----|---------------------------|
| PurchaseID | int          | PK  | Unique purchase ID        |
| VendorName | varchar(45)  |     | Vendor                    |
| PurchaseDate| date        |     | Date of purchase          |
| Amount     | decimal(45,0)|     | Purchase amount           |
| Reimbursed | tinyint      |     | Reimbursement status      |
| RequestID  | int          | FK  | References REQUEST        |

## STAFF
| Column Name | Data Type    | Key | Description           |
|------------|-------------|-----|-----------------------|
| StaffID    | int         | PK  | Unique staff ID       |
| Name       | varchar(45) |     | Staff name            |
| Phone      | varchar(45) |     | Phone number          |
| Title      | varchar(45) |     | Job title             |
| Email      | varchar(45) |     | Email address         |

## STUDENT
| Column Name       | Data Type    | Key | Description                  |
|------------------|-------------|-----|------------------------------|
| StudentID        | int         | PK  | Unique student identifier    |
| Name             | varchar(45) |     | Student name                 |
| PhoneNumber      | varchar(45) |     | Phone number                 |
| UniversityEmail  | varchar(45) |     | University email             |
| ExpectedGradYear | int         |     | Expected graduation year     |

# Section 5 SQL Queries

## Complex 1

Question: How many events has each club hosted?
Why it matters: This helps measure club activity levels and engagement across campus. Clubs hosting more events are often more active and involved in student life. This can also help administrators understand how much funding each club needs.

```
SELECT Club Name, COUNT()
FROM CLUB c
JOIN EVENT HOST e ON c.ClubID = e.CLUB_ClubID
GROUP BY Club Name
ORDER BY COUNT();
```
## Complex 2

Question: What is the average budget amount for each club?
Why it matters: This provides insight into funding distribution and helps compare financial support between clubs over time.
```
SELECT Club Name, AVG(Budget Amount) AS avg_budget
FROM CLUB c
JOIN BUDGET b ON c.ClubID = b.CLUB_ClubID
GROUP BY c.Club Name
ORDER BY avg_budget;
```
## Complex 3

Question: Which students have returned equipment in a different condition at least three times?
Why it matters: This identifies patterns of misuse or damage, which is important for accountability and resource management.
```
SELECT s.Name, COUNT() AS num_returns
FROM STUDENT s
JOIN EQUIPMENT CHECKOUT e
ON e.STUDENT_StudentID = s.StudentID
WHERE e.EQUIPMENT CONDITION_ConditionID <> e.EQUIPMENT CONDITION_ConditionID1
GROUP BY s.StudentID, s.Name
HAVING COUNT() >= 3;
```
## Complex 4

Question: How many students graduating in 2029 are in each club?
Why it matters: This helps understand class year representation in clubs, useful for planning recruitment and leadership continuity.
```
SELECT Club Name, COUNT()
FROM CLUB c
JOIN MEMBERSHIP m ON m.STUDENT_StudentID = c.ClubID
JOIN STUDENT s ON m.STUDENT_StudentID = s.StudentID
WHERE s.Expected Grad Year = 2029
GROUP BY Club Name
ORDER BY COUNT();
```
## Complex 5

Question: Which clubs have high or low budget values?
Why it matters: Categorizing budgets makes it easier to quickly assess which clubs have significant funding and which may need additional support. This can also make it easier for staff adminstrators to know which budgets are more signifcant and may require a more careful look, and which can be expedited
```
SELECT Club Name, IF(Budget Amount > 5000, "High value", "Low value") AS "Value"
FROM CLUB c
JOIN BUDGET b ON c.ClubID = b.CLUB_ClubID;
```
## Complex 6

Question: Which students are members of more than five clubs?
Why it matters: This highlights highly involved students who may be strong candidates for leadership roles or recognition, additionally as * was selected, this allows administrators to conduct further analysis on what types of students are more involved.
```
SELECT * FROM STUDENT s
WHERE (SELECT COUNT(*) FROM MEMBERSHIP m
WHERE m.STUDENT_StudentID = s.StudentID ) > 5;
```
## Simple 1

Question: Which students are members of at least one club?
Why it matters: This shows overall student participation in campus organizations, additionally it can be used to send mass emails.
```
SELECT * FROM STUDENT
WHERE StudentID IN (SELECT STUDENT_StudentID FROM MEMBERSHIP);
```
## Simple 2

Question: Which equipment checkouts resulted in a condition change?
Why it matters: This helps track potential damage or wear to equipment, supporting maintenance and policy decisions, additionally this can allow administratiors to track which equipment may need to be replaced in the future.
```
SELECT * FROM EQUIPMENT CHECKOUT
WHERE EQUIPMENT CONDITION_ConditionID <> EQUIPMENT CONDITION_ConditionID1;
```
## Simple 3

Question: Which students are members of more than one club?
Why it matters: This identifies students with broader campus involvement and cross-club engagement.
```
SELECT STUDENT_StudentID, COUNT()
FROM MEMBERSHIP
GROUP BY STUDENT_StudentID
HAVING COUNT() > 1;
```
## Simple 4

Question: What are the unique equipment item names?
Why it matters: This provides a clear list of available equipment types for inventory tracking and reporting.
```
SELECT DISTINCT Item Name
FROM EQIPMENT;
```

| Query       | GROUP BY | JOIN | 2+ JOINs | HAVING | WHERE | ORDER BY | OTHER |
|------------|----------|------|----------|--------|-------|----------|-----------|
| Simple 1   |          |      |          |        | x     |          | SUBQUEARY |
| Simple 2   |          |      |          |        | x     |          |           |
| Simple 3   | x        |      |          | x      |       |          | AGGREATION|
| Simple 4   |          |      |          |        |       |          | DISTINCT  |
| Complex 1  | x        | x    |          |        |       | x        |           |
| Complex 2  | x        | x    |          |        |       | x        |           |
| Complex 3  | x        | x    |          | x      | x     |          |AGGREGATION|
| Complex 4  | x        | x    | x        |        | x     | x        |           |
| Complex 5  |          | x    |          |        |       |          | IF LOGIC  |
| Complex 6  |          |      |          |        | x     |          | SUBQUEARY |

# Section 6 Database information

all quaries in GP_Qx

Database name - mb_B8
Database pswd - 89898989

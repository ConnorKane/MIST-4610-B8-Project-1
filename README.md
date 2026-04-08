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

# Section 4 Data Dictionary

# Section 5 SQL Queries

## Complex 1

Question: How many events has each club hosted?
Why it matters: This helps measure club activity levels and engagement across campus. Clubs hosting more events are often more active and involved in student life. This can also help administrators understand how much funding each club needs

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
Why it matters: This helps understand class-year representation in clubs, useful for planning recruitment and leadership continuity.
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
Why it matters: Categorizing budgets makes it easier to quickly assess which clubs have significant funding and which may need additional support.
```
SELECT Club Name, IF(Budget Amount > 5000, "High value", "Low value") AS "Value"
FROM CLUB c
JOIN BUDGET b ON c.ClubID = b.CLUB_ClubID;
```
## Complex 6

Question: Which students are members of more than five clubs?
Why it matters: This highlights highly involved students who may be strong candidates for leadership roles or recognition.
```
SELECT * FROM STUDENT s
WHERE (SELECT COUNT(*) FROM MEMBERSHIP m
WHERE m.STUDENT_StudentID = s.StudentID ) > 5;
```
## Simple 1

Question: Which students are members of at least one club?
Why it matters: This shows overall student participation in campus organizations.
```
SELECT * FROM STUDENT
WHERE StudentID IN (SELECT STUDENT_StudentID FROM MEMBERSHIP);
```
## Simple 2

Question: Which equipment checkouts resulted in a condition change?
Why it matters: This helps track potential damage or wear to equipment, supporting maintenance and policy decisions.
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

# Section 6 Database information

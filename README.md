
 Library Management Mini Project & Relational Algebra Task
Project Description
This mini-project demonstrates:

-Database Design (ERD, constraints)

-SQL Development (tables, stored procedures, functions, triggers, indexes)

-performance optimization (indexing)

-Relational Algebra transformation and optimization

 Task 1: Library Management System
 ERD (Entity-Relationship Diagram)
Entities: Books, Members, BorrowedBooks

Constraints:

1 Member can borrow many books.

1 Book can be borrowed many times.

Mandatory participation for BorrowedBooks (both MemberID and BookID must exist).

Avoided fan traps and chasm traps by careful entity-relationship linking.

 [ERD Image Provided]

 SQL Scripts
1. Table Creation
Books (BookID, Title, Author, CopiesAvailable, TotalCopies)

Members (MemberID, Name, Email, TotalBooksBorrowed, IsActive)

BorrowedBooks (BorrowID, MemberID, BookID, BorrowDate, DueDate, ReturnDate, IsReturned)

2. Stored Procedure: BorrowBook
-Checks if member is active

-Checks if book has available copies

-Inserts borrowing record (IsReturned = false)

-Updates CopiesAvailable and TotalBooksBorrowed

3. Index Creation
-Index on Books(BookID)

-Index on Members(MemberID)


4. Function: GetBooksBorrowed
Scalar function that returns how many books are currently borrowed and not yet returned by a specific MemberID.

5. Trigger: PreventBorrowIfNoCopies
BEFORE INSERT trigger on BorrowedBooks.

Prevents insertion if:

-CopiesAvailable = 0

-Member IsActive = false

-Raises clear error messages.

6. Sample Data Insertion
Insert at least:

3 Books

3 Members

5 Borrowing attempts

 Task 2: SQL to Relational Algebra
 Initial Relational Algebra Expression

 code
π_{S.Name, C.Title, E.Grade} 
(σ_{S.Major = 'Computer Science' ∧ C.Credits ≥ 3 ∧ S.StudentID = E.StudentID ∧ C.CourseID = E.CourseID}
 (STUDENT S × COURSE C × ENROLLMENT E))

 Query Tree
First: Cartesian product of STUDENT × COURSE × ENROLLMENT

Then: Apply selection (σ)

Then: Apply projection (π)

 Optimized Query Plan

Code
π_{S.Name, C.Title, E.Grade}
(
  (σ_{S.Major='Computer Science'}(STUDENT)) 
    ⋈_{S.StudentID = E.StudentID}
  (ENROLLMENT ⋈_{E.CourseID = C.CourseID} σ_{C.Credits≥3}(COURSE))
)
Optimization Steps
Push Selections Down: Apply σ before joins to reduce tuple sizes early.

Replace Cartesian Product + Selection with Joins: Merge cross-product and condition into efficient joins.

Early Projection: After joins, project only necessary attributes (Name, Title, Grade).

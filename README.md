# Registration System

A university course registration system designed for administrators, lecturers, and students using only core Java

## Overview

This system provides a solution for managing university course registrations with role-based access control. This system fulfills all requirements of the ITE222 course final project. In addition, it verifies students majors to provide the correct curriculum and manages free elective and transferred courses for students who change majors

## Features

- **Multi-Role Support**:Distinct features and permissions for Administrators, Lecturers, and Students

- **Registration Management**: Prevents invalid registrations, enforces prerequisites/credits,... controls course capacity, and supports graduation checks

- **Curriculum Management**: Matches students majors with the correct curriculum and handles free elective and transferred courses for students who change majors
  
- **Term Scheduling**: Manage course schedules and seats limit

## How to run

1. Find the usernames and passwords in `IDsPasswords.java`
2. Run `Main.java`
3. Sample student data (Student ID to be searched) can be found in `StudentFunctions.java`

* Note: The system expects users to enter the correct data types (e.g., integers or strings) as prompted. If an incorrect data type is entered, the program may terminate, which is an expected behavior for this console-based implementation

---------------------------------------------------
## Class Descriptions

## Student.java

- Uses object-oriented encapsulation to model student data by grouping related attributes into a single class and restricting direct access through private fields
- Applies constructor-based initialization and getter methods

## StudentFunctions.java

- Applies rule-based validation for registration (completed-course checks, prerequisites, credit requirements, course limit, time conflict, and seat availability)
- Applies constructors to initialize system objects
- Uses methods to organize student operations such as viewing records, registering or dropping courses, and checking prerequisites

## PrintProgress.java
- Uses method overloading (printProgress(...)) to print progress in different formats (by curriculum category vs. free electives)
- Applies loops and conditional logic (for, if, switch) to find students, count completed vs. remaining courses, and display course status (COMPLETED/TRANSFERRED/PENDING) with prerequisite checks.

## Course.java

- The AdminFunctions class act like manager for course and system data in the university registration system and it declare several private static field. Example are 'courseCode', 'courseName', 'section', 'timeSlot', 'seat', and 'category' which are used to store data for different operations
- To handle different tasks, the class use the menu method to display options and the selection method with if-else allowing the admin to choose between adding, removing, or opening courses depending on their purpose
- Example is the addCourses method is use to interact with the Curriculum class using newCourses to add a new course object to the system while removeCourses call deleteCourses in Curriculum to remove the course based on course code
- Another for editSeats to interact with the TermSchedule class to update or set the number of seat limit. Whereas openCourse add a new offering to the term schedule by calling termSchedule.openCourse
- The readTrack() uses a switch statement to show user input to categories and a while loop to ensure the admin's choice is valid before the system attempts to modify any data

## TermSchedule.java

- The TermSchedule class for available class offerings in the university registration system and it declare a private static List called schedule.

- Example is the schedule which is an ArrayList storing new Course objects created with data like code, section, timeSlot, and seats. To handle different tasks, the class includes several public methods like: 
  
  • printSchedule(): This method is use to display the term schedule and it uses a for-each loop to print every offering's code, section, timeSlot, and seats.
  
  • editSeats(String code ,...): When the admin chooses to Set courses seat limits, the system calls editSeats in AdminFunctions, which then calls the same method in TermSchedule to modify or update the seats field for a specific course
  
  • openCourse(String code, int section ...): To handle the task of adding a new class, AdminFunctions takes user input and calls openCourse in TermSchedule to add a new Course object to the ArrayList
  
  • closeCourse(String code, int section): Another for closeCourse where AdminFunctions calls this method to use removeIf and delete a specific class offering from the schedule based on its code and section. 
  
  • systemStatistics(): When the admin selects View system statistics, AdminFunctions calls viewReport(), which triggers this method in TermSchedule to calculate and display totalCourses using size() and use a loop to sum the totalSeats to find the averageSeats per course.

## AdminFunctions.java

The AdminFunctions class act like manager for course and system data in the university registration system and it declare several private static field. Example are 'courseCode', 'courseName', 'section', 'timeSlot', 'seat', and 'category' which are used to store data for different operations. 

- To handle different tasks, the class use the menu method to display options and the selection method with if-else allowing the admin to choose between adding, removing, or opening courses depending on their purpose.

- Example is the addCourses method is use to interact with the Curriculum class using newCourses to add a new course object to the system while removeCourses call deleteCourses in Curriculum to remove the course based on course code. 

- Another for editSeats to interact with the TermSchedule class to update or set the number of seat limit. Whereas openCourse add a new offering to the term schedule by calling termSchedule.openCourse. 

- The readTrack() uses a switch statement to show user input to categories and a while loop to ensure the admin's choice is valid before the system attempts to modify any data. 

## AdvisorFunctions.java

- The AdvisorFunctions class provides the interface and algorithms for the advisor to manage students. It interacts heavily with the StudentFunctions and Curriculum classes.

When an advisor enters a Student ID, the system performs a lookup in the global student database.
*  Method Used: searchStudent(String studentID) in AdvisorFunctions.
*  Internal Workflow: It acts as a bridge, calling studentRecord.printCompletedCourse(studentID) inside the StudentFunctions class. It looks through the student list, finds the matching ID, and prints their grade, major, and every course code they have successfully finished.

Advisor can register or withdraw students from specific courses.
*  Method Used: viewProgress(String studentID) in AdvisorFunctions.
*  Internal Workflow: It calls the method checkMajorAndPrint(studentID) from PrintProgress class. It compares the student's completedCourses list against the Curriculum categories. 
*  It also handles Equivalency Logic: if a student took an IT course that is equivalent to a CS course, the code marks the requirement as met by checking the Equivalency category in the Curriculum list.
 
When an advisor selects the "View Graduation Risks" option, the following logic is executed:
* The system loops through every student record stored in StudentFunctions
The graduationRisks() method calculates a student's courses taken per term to assign a risk level:

*  HIGH RISK: Student is taking fewer than 2 courses per term.

*  MEDIUM RISK: Student is taking fewer than 4 courses per term.

*  LOW RISK: Student is taking 4 or more courses per term (on track for 4-year graduation).

## Curriculum.java

The Curriculum class acts as the central database for the university registration system. It manages academic data which includes course offerings, prerequisites, and credit rules.

Data Storage
* Static List: All subjects are stored in a private static ArrayList<Course> called course.

* Course Objects: This list stores various types of course data depending on the initialization:

* Standard Subjects: Initializes category, code, and name.

* Prerequisites: Includes a String[] of required codes (e.g., must finish ENG101 to attend ENG102).

* Equivalency: Maps codes between different departments (e.g., linking ITE codes with equivalent BSC/CSC codes).

Methods used
* getCourse(): Returns the entire list of courses to the calling system.

* printCurriculum(): reads through the list using a for-each loop and if statements to filter and display the code and name for specific categories.

* newCourses(String category, String code, String name): Adds a new Course object to the ArrayList and automatically triggers printCurriculum() to show the update.

* deleteCourses(category, code): Searches the list and removes any entry where both the category and code match the user input.

Constants & Rules
* TOTAL_CREDITS: 160

* COURSE_CREDITS: 4 per course

* INTERN_CREDITS: 12

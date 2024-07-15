# University-Management-System (UMS)

Overview
The UMS is designed to streamline the operations of a university by managing user details, courses, and administrative processes through a unified platform. This system will support various functionalities to add, modify, retrieve, and manage user data across different departments. This system utilizes core object-oriented principles such as inheritance, encapsulation, polymorphism, and the use of collections like ArrayList and HashMap to efficiently manage data and operations.
Main Functionalities:
1.	User Login and Authentication: Users can log in with their credentials to access different functionalities based on their role (Student, Professor, Admin).
2.	Manage User Details: Add new users, update existing user information, and remove users from the system.
3.	Display User Details: Retrieve and display details of users, including students, professors, and administrators.
4.	Course and Grade Management: Professors can manage course details and student grades.
5.	Administrative Functions: Admins can perform administrative tasks such as managing user roles, viewing all registered users, and adjusting salaries.





Use Cases:
1.	Adding a User:
•	Actor: Admin
•	Precondition: Admin is logged in.
•	Main Flow: The admin selects the option to add a user, enters user details (name, ID, department, contact, role), and submits the information. The system validates the data and adds the user to the database.
•	Postcondition: The user is added to the system.

2.	Removing a User:
•	Actor: Admin
•	Precondition: Admin is logged in and the user exists.
•	Main Flow: The admin selects the remove user option, searches for the user by ID, and confirms the deletion.
•	Postcondition: The user is removed from the system.

3.	Updating User Details:
•	Actor: Admin
•	Precondition: Admin is logged in and the user exists.
•	Main Flow: The admin selects the update user option, modifies the necessary details, and submits the changes.
•	Postcondition: User details are updated in the system.

4.	Viewing User Details:
•	Actor: All users
•	Precondition: User is logged in.
•	Main Flow: The user selects the option to view details, and the system displays their information.
•	Postcondition: User details are displayed.

5.	Managing Courses and Grades:
•	Actor: Professor
•	Precondition: Professor is logged in.
•	Main Flow: The professor selects the course management option, enters or updates course details and student grades.
•	Postcondition: Course information and grades are updated.
Special Requirements:
•	The system must handle concurrent logins and operations without data integrity issues.
•	Sensitive data like user passwords and contact information must be encrypted and securely stored.
Technology and Data:
•	Programming Language: Java
•	Data Structures: Lists for storing user data, maps for credentials, and enums for roles and departments.
•	User Interface: Console-based with text inputs for simplicity and ease of testing.

Design Decisions
1. Use of Enumerations (enum):
•	Purpose: To ensure the integrity and consistency of user types (UserType) and departments (Department) throughout the application.
•	Benefits: Reduces errors by limiting the options to predefined constants, thereby simplifying the management of user roles and department affiliations.
2. Class Structure:
•	User Class (Parent):
•	Attributes: Common attributes like name, userID, department, and contact number are defined here.
•	Methods: Includes a method to display user details which is inherited by child classes.
•	Inheritance: Acts as a base class for Student, Professor, and Admin.
•	Reason: Promotes code reusability and simplifies modifications.
•	Student, Professor, Admin (Children):
•	Specialization: Each class extends User and adds specific attributes relevant to their roles.
•	Polymorphism: The displayDetails method is overridden in Professor to include additional attributes like course and salary.
•	Encapsulation: All attributes are private with selective visibility through methods.
3. System Architecture:
•	Data Handling: Utilizes ArrayList for storing user objects and HashMap for managing credentials, demonstrating effective use of collections.
•	Input Handling: The Scanner class is employed to handle user input, ensuring robust data intake and processing.

Functional Specifications
•	Login System: Validates user credentials against stored data, ensuring secure access to system functionalities.
•	User Actions: Differentiates actions based on user type, such as viewing details, managing students, and handling administrative tasks.
•	Dynamic User Interaction: Users can add, modify, or delete information dynamically, enhancing the flexibility of the system.

Challenges and Solutions
1. Handling User Input:
•	Challenge: Managing user input errors and ensuring smooth flow within the system.
•	Solution: Implemented comprehensive error checking and used scanner.nextLine() after nextInt() to capture newline characters, preventing input mismatches.
2. Extending Functionality:
•	Challenge: Extending system capabilities without altering existing code significantly.
•	Solution: Used inheritance to extend the User class, allowing easy addition of new user types and attributes without impacting existing functionality.
3. Designing for Scalability:
•	Challenge: Ensuring the system could handle an increasing number of users and interactions as the university grows.
•	Solution: Employed scalable collections like ArrayList and HashMap, which adjust their size dynamically, accommodating more data as needed.

# University-Management-System

import java.util.ArrayList; // Import the ArrayList class for dynamic array manipulation.
import java.util.HashMap; // Import the HashMap class for storing key-value pairs.
import java.util.List; // Import the List interface to use as a type for object lists.
import java.util.Map; // Import the Map interface for mapping keys to values.
import java.util.Scanner; // Import the Scanner class to read input from various sources.
import java.util.stream.Collectors; // Import Collectors utility to perform operations on streams.

// Define UserType as an enum to specify different user roles.
enum UserType {
    STUDENT, PROFESSOR, ADMIN
}

// Define Department as an enum to specify departments within the university.
enum Department {
    ADMIN, IT, DESIGN, BUSINESS, ENGINEERING;
}

// Parent class User defines common properties and methods for all user types.
class User {
    String name; // Store the user's name.
    String userId; // Store a unique user identifier.
    Department department; // Store the department associated with the user.
    String contactNumber; // Store the user's contact number.
    UserType userType; // Store the type of user from the UserType enum.

    // Constructor initializes a new User object with provided details.
    User(String name, String userId, Department department, String contactNumber, UserType userType) {
        this.name = name;
        this.userId = userId;
        this.department = department;
        this.contactNumber = contactNumber;
        this.userType = userType;
    }
    
    // Method to display user details in a structured format.
    void displayDetails() {
        System.out.println("------------------------------");
        System.out.println("Name: " + name);
        System.out.println("UserID: " + userId);
        System.out.println("Department: " + department);
        System.out.println("Contact Number: " + contactNumber);
        System.out.println("User Type: " + userType);
        System.out.println("------------------------------");
    }
}

// Student class extends User, adding a grade property.
class Student extends User {
    double grade; // Store the student's grade.

    // Constructor initializes a Student object, including the grade.
    Student(String name, String userId, Department department, String contactNumber, double grade) {
        super(name, userId, department, contactNumber, UserType.STUDENT); // Call the superclass constructor.
        this.grade = grade; // Set the student's grade.
    }
}

// Professor class extends User, adding course and annual salary properties.
class Professor extends User {
    String course; // Store the name of the course the professor teaches.
    double annualSalary; // Store the professor's annual salary.

    // Constructor initializes a Professor object, including course and salary.
    Professor(String name, String userId, Department department, String contactNumber, String course, double annualSalary) {
        super(name, userId, department, contactNumber, UserType.PROFESSOR); // Call the superclass constructor.
        this.course = course; // Set the course name.
        this.annualSalary = annualSalary; // Set the annual salary.
    }

    // Override the displayDetails method to include course and salary details.
    @Override
    void displayDetails() {
        super.displayDetails(); // Call the superclass method.
        System.out.println("Course: " + course); // Display the course name.
        System.out.println("Annual Salary: " + annualSalary); // Display the salary.
    }
}

// Admin class extends User without adding new properties.
class Admin extends User {
    // Constructor initializes an Admin object.
    Admin(String name, String userId, Department department, String contactNumber) {
        super(name, userId, department, contactNumber, UserType.ADMIN); // Call the superclass constructor.
    }   
    
}

// Main class UMS handles the university management system operations.
public class UMS {
    private List<User> users = new ArrayList<>(); // List to store all users.
    private Map<String, String> credentials = new HashMap<>(); // Map to store user credentials.
    private Scanner scanner = new Scanner(System.in); // Scanner object for input operations.
    
    // Constructor initializes system with default users and credentials.
    UMS() {
        // Initialize system with predefined users.
        users.add(new Student("Hyeongjun (Jun) Jeon", "IS1", Department.IT, "111-222-333", 3.5));
        users.add(new Student("Leane Labelle", "DS1", Department.DESIGN, "121-232-313", 4.2));
        users.add(new Student("Aaron Urayan", "BS1", Department.BUSINESS, "123-132-317", 2.2));
        users.add(new Student("Clarice Ong", "ES1", Department.ENGINEERING, "161-747-774", 3.9));

        users.add(new Professor("Kelvin Wang", "IP1", Department.IT, "444-555-666", "IPRO002", 50000.00));
        users.add(new Professor("James Hu", "IP2", Department.IT, "321-211-221", "DATABASE AND SQL", 52000.00));
        users.add(new Professor("Virginie Labelle", "DP1", Department.DESIGN, "828-282-848", "DESIGN001", 48000.00));
        users.add(new Professor("Benjamin Franklin", "BP1", Department.BUSINESS, "828-121-848", "BUSINESS FUNDAMENTAL", 71000.00));
        users.add(new Professor("Timothy Ling", "EP1", Department.ENGINEERING, "482-223-175", "PROGRAMMING FOR ENGINEERING", 62000.00));
        users.add(new Admin("Admin Mcdonald", "A1", Department.ADMIN, "777-888-999"));

        // Set default credentials for all users.
        credentials.put("IS1", "studentpass");
        credentials.put("DS1", "studentpass");
        credentials.put("BS1", "studentpass");
        credentials.put("ES1", "studentpass");
        credentials.put("IP1", "professorpass");
        credentials.put("IP2", "professorpass");
        credentials.put("DP1", "professorpass");
        credentials.put("BP1", "professorpass");
        credentials.put("EP1", "professorpass");
        credentials.put("A1", "adminpass");
    }

    // Main method initializes the UMS system and starts the user interaction loop.
    public static void main(String[] args) {
        UMS ums = new UMS(); // Create an instance of UMS.
        ums.runSystem(); // Start the main interaction loop of the system.
    }

    // runSystem handles the main loop, prompting for login or system exit.
    private void runSystem() {
        while (true) {
            System.out.println("\n*** University Management System ***");
            System.out.println("1. Login\n2. Exit");
            System.out.print("Choose an option: ");
            int option = scanner.nextInt();
            scanner.nextLine();  // Consume newline left-over to prevent input errors.

            switch (option) {
                case 1:
                    login(); // Handle user login.
                    break;
                case 2:
                    System.out.println("Exiting..."); // Exit the application.
                    return;
                default:
                    System.out.println("Invalid option, please try again."); // Handle incorrect input.
            }
        }
    }

    // login method handles user login using credentials stored in the system.
    private void login() {
        System.out.print("Enter User ID: ");
        String userId = scanner.nextLine();
        System.out.print("Enter Password: ");
        String password = scanner.nextLine();

        if (credentials.containsKey(userId) && credentials.get(userId).equals(password)) {
            for (User user : users) {
                if (user.userId.equals(userId)) {
                    System.out.println("Login successful."); // Confirm successful login.
                    userActions(user); // Direct to user-specific actions.
                    return;
                }
            }
        } else {
            System.out.println("Invalid credentials."); // Handle failed login attempt.
        }
    }

    // userActions dispatches actions based on the logged-in user's type.
    private void userActions(User user) {
        user.displayDetails(); // Display details of the logged-in user.
        switch (user.userType) {
            case STUDENT:
                studentActions((Student) user); // Handle actions specific to students.
                break;
            case PROFESSOR:
                professorActions((Professor) user); // Handle actions specific to professors.
                break;
            case ADMIN:
                adminActions(); // Handle actions specific to administrators.
                break;
        }
    }

    // studentActions provides options specific to student users.
    private void studentActions(Student student) {
        int option = 0;
        do {
            System.out.println("1. My Details"); // Option to display student details.
            System.out.println("2. Department"); // Option to display student's department.
            System.out.println("3. Log out"); // Option to log out.
            System.out.print("Choose an option: ");
            option = scanner.nextInt();
            scanner.nextLine();  // Consume newline left-over to prevent input errors.

            switch (option) {
                case 1:
                    student.displayDetails(); // Display details of the student.
                    break;
                case 2:
                    System.out.println("------------------------------");
                    System.out.println("Department: " + student.department + ", GPA: " + student.grade); // Display department and GPA.
                    System.out.println("------------------------------");
                    break;
                case 3:
                    System.out.println("Logging out..."); // Handle logout.
                    break;
                default:
                    System.out.println("Invalid option, please try again."); // Handle incorrect input.
            }
        } while (option != 3);
    }

    // professorActions provides options specific to professor users.
    private void professorActions(Professor professor) {
        int option = 0;
        do {
            System.out.println("1. My Details"); // Option to display professor details.
            System.out.println("2. Department"); // Option to display professor's department.
            System.out.println("3. My Students"); // Option to manage professor's students.
            System.out.println("4. Log Out"); // Option to log out.
            System.out.print("Choose an option: ");
            option = scanner.nextInt();
            scanner.nextLine();  // Consume newline left-over to prevent input errors.

            switch (option) {
                case 1:
                    professor.displayDetails(); // Display details of the professor.
                    break;
                case 2:
                    System.out.println("------------------------------");
                    System.out.println("Department: " + professor.department); // Display the department of the professor.
                    System.out.println("------------------------------");
                    break;
                case 3:
                    displayStudents(professor); // Show students associated with the professor.
                    break;
                case 4:
                    System.out.println("Logging out..."); // Handle logout.
                    break;
                default:
                    System.out.println("Invalid option, please try again."); // Handle incorrect input.
            }
        } while (option != 4);
    }
    
    
    private void displayStudents(Professor professor) {
        List<Student> students = users.stream()
            .filter(user -> user instanceof Student && user.department.equals(professor.department)) // Filter students from the same department as the professor.
            .map(user -> (Student) user) // Cast filtered users to Student type.
            .collect(Collectors.toList()); // Collect results into a list.
        
        if (students.isEmpty()) {
            System.out.println("No students found in your department."); // Handle case where no students are found.
            return;
        }
    
        students.forEach(student -> {
            System.out.println(student.name + " - GPA: " + student.grade); // Display each student's name and GPA.
        });
    
        System.out.println("0. Change GPA");
        System.out.print("Choose an option: ");
        int option = scanner.nextInt();
        scanner.nextLine();  // Consume newline left-over to prevent input errors.
    
        if (option == 0) {
            System.out.print("Enter Student ID: ");
            String studentId = scanner.nextLine();
            Student selectedStudent = students.stream()
                .filter(student -> student.userId.equals(studentId)) // Find the specific student by ID.
                .findFirst()
                .orElse(null); // Return null if no matching student is found.
    
            if (selectedStudent == null) {
                System.out.println("Student not found."); // Handle case where the student isn't found.
                return;
            }
    
            System.out.print("Enter new GPA: ");
            double newGpa = scanner.nextDouble();
            scanner.nextLine();  // Consume newline left-over to prevent input errors.
    
            selectedStudent.grade = newGpa; // Update the student's GPA.
            System.out.println("GPA updated successfully."); // Confirm GPA update.
        }
    }
    
    // adminActions provides options specific to administrator users.
    private void adminActions() {
        int option;
        do {
            System.out.println("\n*** Admin Menu ***"); // Display admin-specific options.
            System.out.println("1. My detail"); // Option to view admin details.
            System.out.println("2. View all Users"); // Option to view all users.
            System.out.println("3. Add user"); // Option to add a new user.
            System.out.println("4. Remove user"); // Option to remove an existing user.
            System.out.println("5. Finance"); // Finance-related options.
            System.out.println("6. Log out"); // Option to log out.
            System.out.print("Choose an option: ");
            option = scanner.nextInt();
            scanner.nextLine();  // Consume newline left-over to prevent input errors.

            switch (option) {
                case 1:
                    displayAdminDetails(); // Display details of the logged-in admin.
                    break;
                case 2:
                    viewAllUsers(); // Display details of all users.
                    break;
                case 3:
                    addUser(); // Add a new user to the system.
                    break;
                case 4:
                    removeUser(); // Remove an existing user from the system.
                    break;
                case 5:
                    financeActions(); // Handle finance-related tasks.
                    break;
                case 6:
                    System.out.println("Logging out..."); // Handle logout.
                    break;
                default:
                    System.out.println("Invalid option, please try again."); // Handle incorrect input.
            }
        } while (option != 6);
    }

    // View details of all registered users
    private void viewAllUsers() {
        System.out.println("Viewing all registered users..."); // Message indicating that all user details are being displayed.
        for (User user : users) {
            user.displayDetails();  // Display details for each user using the inherited method
        }
    }
    
    // Method to add a new user to the system with back to menu functionality
    private void addUser() {
        System.out.println("Enter user type (1-Student, 2-Professor, 3-Admin, 0-Back): ");
        String input = scanner.nextLine(); // Read the whole line for checking back option
        if (input.equalsIgnoreCase("0")) { // Check if the user wants to go back
            return; // Return to main menu
        }

        int type;
        try {
            type = Integer.parseInt(input); // Parse the input to an integer
        } catch (NumberFormatException e) {
            System.out.println("Invalid input. Please enter a valid number.");
            return;
        }

        System.out.print("Name: ");
        String name = scanner.nextLine();
        if (name.equalsIgnoreCase("0")) return; // Check if the user wants to go back

        System.out.print("User ID: ");
        String userId = scanner.nextLine();
        if (userId.equalsIgnoreCase("0")) return; // Check if the user wants to go back

        System.out.print("Department: ");
        String deptInput = scanner.nextLine().toUpperCase();
        if (deptInput.equalsIgnoreCase("0")) return; // Check if the user wants to go back

        Department department;
        try {
            department = Department.valueOf(deptInput);
        } catch (IllegalArgumentException e) {
            System.out.println("Invalid department. Please enter a valid department.");
            return;
        }

        System.out.print("Contact Number: ");
        String contactNumber = scanner.nextLine();
        if (contactNumber.equalsIgnoreCase("0")) return; // Check if the user wants to go back

        System.out.print("Password: ");
        String password = scanner.nextLine();
        if (password.equalsIgnoreCase("0")) return; // Check if the user wants to go back

        // Depending on the user type, different information might be required
        switch (type) {
            case 1:
                System.out.print("Grade: ");
                if (scanner.hasNextDouble()) {
                    double grade = scanner.nextDouble();
                    scanner.nextLine(); // Consume the newline left-over
                    users.add(new Student(name, userId, department, contactNumber, grade));
                } else {
                    scanner.nextLine(); // Consume the newline and invalid input
                    System.out.println("Invalid input for grade. Returning to menu.");
                    return;
                }
                break;
            case 2:
                System.out.print("Course: ");
                String course = scanner.nextLine(); // Read the course directly
                if (course.equalsIgnoreCase("0")) return; // Check if the user wants to go back
                System.out.print("Annual Salary: ");
                if (scanner.hasNextDouble()) {
                    double annualSalary = scanner.nextDouble();
                    scanner.nextLine(); // Consume the newline left-over
                    users.add(new Professor(name, userId, department, contactNumber, course, annualSalary));
                } else {
                    scanner.nextLine(); // Consume the newline and invalid input
                    System.out.println("Invalid input for salary. Returning to menu.");
                    return;
                }
                break;
            case 3:
                users.add(new Admin(name, userId, department, contactNumber));
                break;
            default:
                System.out.println("Invalid user type. Returning to main menu.");
                return;
        }
        credentials.put(userId, password); // Store the new user's credentials
        System.out.println("User added successfully.");
    }


    // Method to remove a user with back to menu option
    private void removeUser() {
        System.out.println("Enter user type (1-Student, 2-Professor, 3-Admin, 0-Back): ");
        String userTypeInput = scanner.nextLine(); // Read the line for input
        if (userTypeInput.equalsIgnoreCase("0")) return; // Check if the user wants to go back

        int userType;
        try {
            userType = Integer.parseInt(userTypeInput);
        } catch (NumberFormatException e) {
            System.out.println("Invalid input. Please enter a valid number.");
            return;
        }

        List<User> usersOfType = users.stream()
            .filter(user -> user.userType.ordinal() == userType - 1)
            .collect(Collectors.toList());

        if (usersOfType.isEmpty()) {
            System.out.println("No users found of this type.");
            return;
        }

        System.out.println("Available users:");
        usersOfType.forEach(user -> System.out.println("UserID: " + user.userId + " - Name: " + user.name));
        System.out.println("Enter User ID to remove (type '0' to go back):");
        String userId = scanner.nextLine();
        if (userId.equalsIgnoreCase("0")) return; // Check if the user wants to go back

        User userToRemove = usersOfType.stream()
            .filter(user -> user.userId.equals(userId))
            .findFirst()
            .orElse(null);

        if (userToRemove == null) {
            System.out.println("User not found.");
            return;
        }

        System.out.print("Confirm removal by entering admin password (type '0' to cancel): ");
        String password = scanner.nextLine();
        if (password.equalsIgnoreCase("0")) return; // Check if the user wants to go back

        if (password.equals(credentials.get("A1"))) {
            users.remove(userToRemove);
            credentials.remove(userToRemove.userId);
            System.out.println("User removed successfully.");
        } else {
            System.out.println("Invalid password. User not removed.");
        }
    }


    // Additional methods for handling administrative tasks like viewing and adjusting professor salaries
    private void financeActions() {
        System.out.println("1. View Annual Salary (Professor)"); // Option to view annual salaries of professors.
        System.out.println("2. Salary Adjustment (Professor)"); // Option to adjust salaries of professors.
        System.out.print("Choose an option: ");
        int option = scanner.nextInt();
        scanner.nextLine(); // Consume newline left-over to prevent input errors.
    
        switch (option) {
            case 1:
                viewProfessorsSalary(); // Display salaries of all professors.
                break;
            case 2:
                adjustProfessorSalary(); // Adjust the salary of a specific professor.
                break;
            default:
                System.out.println("Invalid option, please try again."); // Handle incorrect input.
        }
    }

    // View annual salaries for professors
    private void viewProfessorsSalary() {
        System.out.println("Professors and their Annual Salaries:"); // Display header for the salary listing.
        for (User user : users) {
            if (user instanceof Professor) {
                Professor professor = (Professor) user;
                System.out.println("Professor Name: " + professor.name + ", Department: " + professor.department + ", Annual Salary: $" + professor.annualSalary); // Display each professor's name, department, and salary.
            }
        }
    }
    
     // Adjust the annual salary of a professor   
    private void adjustProfessorSalary() {
        System.out.print("Enter Professor ID: "); // Prompt for the ID of the professor whose salary is to be adjusted.
        String profId = scanner.nextLine();
        Professor professor = null;
    
        // Find the professor by ID and adjust their salary
        for (User user : users) {
            if (user instanceof Professor && user.userId.equals(profId)) {
                professor = (Professor) user;
                break;
            }
        }
    
        if (professor == null) {
            System.out.println("Professor not found."); // Handle case where the professor isn't found.
            return;
        }
    
        System.out.print("Enter the new annual salary for " + professor.name + ": "); // Prompt for the new salary amount.
        double newSalary = scanner.nextDouble();
        scanner.nextLine();  // Consume the newline left-over to prevent input errors.
    
        professor.annualSalary = newSalary; // Update the professor's salary.
        System.out.println("The annual salary of Professor " + professor.name + " has been adjusted to $" + newSalary); // Confirm the salary adjustment.
    }

    // Display details of the admin who is currently logged in
    private void displayAdminDetails() {
        System.out.println("Displaying admin details..."); // Message indicating that admin details are being displayed.
        // Assuming the current user is an admin, loop through users to find and display the admin's details.
        for (User user : users) {
            if (user instanceof Admin) {
                user.displayDetails();  // Admin's details displayed in line-by-line format as per User class implementation.
                break;
            }
        }
    }

    
}

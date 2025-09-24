import java.util.*;

// Student class to represent individual student records
class Student {
    private int id;
    private String name;
    private double marks;
    
    // Constructor
    public Student(int id, String name, double marks) {
        this.id = id;
        this.name = name;
        this.marks = marks;
    }
    
    // Getters
    public int getId() {
        return id;
    }
    
    public String getName() {
        return name;
    }
    
    public double getMarks() {
        return marks;
    }
    
    // Setters
    public void setId(int id) {
        this.id = id;
    }
    
    public void setName(String name) {
        this.name = name;
    }
    
    public void setMarks(double marks) {
        this.marks = marks;
    }
    
    // toString method for displaying student information
    @Override
    public String toString() {
        return String.format("ID: %-5d | Name: %-20s | Marks: %.2f", id, name, marks);
    }
}

// Main class for Student Record Management System
public class StudentRecordManagementSystem {
    private static ArrayList<Student> students = new ArrayList<>();
    private static Scanner scanner = new Scanner(System.in);
    private static int nextId = 1;
    
    public static void main(String[] args) {
        System.out.println("=== Student Record Management System ===");
        System.out.println("Welcome to the CLI-based CRUD system!");
        
        // Add some sample data
        initializeSampleData();
        
        while (true) {
            displayMenu();
            int choice = getChoice();
            
            switch (choice) {
                case 1:
                    addStudent();
                    break;
                case 2:
                    viewAllStudents();
                    break;
                case 3:
                    updateStudent();
                    break;
                case 4:
                    deleteStudent();
                    break;
                case 5:
                    searchStudent();
                    break;
                case 6:
                    System.out.println("Thank you for using Student Record Management System!");
                    System.exit(0);
                    break;
                default:
                    System.out.println("Invalid choice! Please select a valid option.");
            }
            
            System.out.println("\nPress Enter to continue...");
            scanner.nextLine();
        }
    }
    
    // Initialize some sample data
    private static void initializeSampleData() {
        students.add(new Student(nextId++, "MS Dhoni", 85.5));
        students.add(new Student(nextId++, "Suresh Raina", 92.3));
        students.add(new Student(nextId++, "Dewald Brevis", 78.9));
        students.add(new Student(nextId++, "Gaurav ", 80.60));
        students.add(new Student(nextId++, "Vedant", 84.70));
    }
    
    // Display the main menu
    private static void displayMenu() {
        System.out.println("\n" + "=".repeat(50));
        System.out.println("           STUDENT RECORD MANAGEMENT");
        System.out.println("=".repeat(50));
        System.out.println("1. Add Student (Create)");
        System.out.println("2. View All Students (Read)");
        System.out.println("3. Update Student (Update)");
        System.out.println("4. Delete Student (Delete)");
        System.out.println("5. Search Student");
        System.out.println("6. Exit");
        System.out.println("=".repeat(50));
        System.out.print("Enter your choice (1-6): ");
    }
    
    // Get user choice with input validation
    private static int getChoice() {
        try {
            int choice = scanner.nextInt();
            scanner.nextLine(); // Consume newline
            return choice;
        } catch (InputMismatchException e) {
            scanner.nextLine(); // Clear invalid input
            return -1;
        }
    }
    
    // CREATE: Add a new student
    private static void addStudent() {
        System.out.println("\n--- ADD NEW STUDENT ---");
        
        System.out.print("Enter student name: ");
        String name = scanner.nextLine().trim();
        
        if (name.isEmpty()) {
            System.out.println("Error: Name cannot be empty!");
            return;
        }
        
        System.out.print("Enter student marks (0-100): ");
        try {
            double marks = scanner.nextDouble();
            scanner.nextLine(); // Consume newline
            
            if (marks < 0 || marks > 100) {
                System.out.println("Error: Marks must be between 0 and 100!");
                return;
            }
            
            Student newStudent = new Student(nextId++, name, marks);
            students.add(newStudent);
            System.out.println("✓ Student added successfully!");
            System.out.println("Student Details: " + newStudent);
            
        } catch (InputMismatchException e) {
            System.out.println("Error: Please enter valid marks!");
            scanner.nextLine(); // Clear invalid input
        }
    }
    
    // READ: View all students
    private static void viewAllStudents() {
        System.out.println("\n--- ALL STUDENT RECORDS ---");
        
        if (students.isEmpty()) {
            System.out.println("No students found in the system.");
            return;
        }
        
        System.out.println("Total Students: " + students.size());
        System.out.println("-".repeat(55));
        
        for (Student student : students) {
            System.out.println(student);
        }
        System.out.println("-".repeat(55));
    }
    
    // UPDATE: Update student information
    private static void updateStudent() {
        System.out.println("\n--- UPDATE STUDENT ---");
        
        if (students.isEmpty()) {
            System.out.println("No students found in the system.");
            return;
        }
        
        System.out.print("Enter student ID to update: ");
        try {
            int id = scanner.nextInt();
            scanner.nextLine(); // Consume newline
            
            Student student = findStudentById(id);
            if (student == null) {
                System.out.println("Error: Student with ID " + id + " not found!");
                return;
            }
            
            System.out.println("Current Details: " + student);
            System.out.println("\nWhat would you like to update?");
            System.out.println("1. Name");
            System.out.println("2. Marks");
            System.out.println("3. Both");
            System.out.print("Enter choice (1-3): ");
            
            int updateChoice = scanner.nextInt();
            scanner.nextLine(); // Consume newline
            
            switch (updateChoice) {
                case 1:
                    System.out.print("Enter new name: ");
                    String newName = scanner.nextLine().trim();
                    if (!newName.isEmpty()) {
                        student.setName(newName);
                        System.out.println("✓ Name updated successfully!");
                    } else {
                        System.out.println("Error: Name cannot be empty!");
                    }
                    break;
                    
                case 2:
                    System.out.print("Enter new marks (0-100): ");
                    double newMarks = scanner.nextDouble();
                    scanner.nextLine();
                    if (newMarks >= 0 && newMarks <= 100) {
                        student.setMarks(newMarks);
                        System.out.println("✓ Marks updated successfully!");
                    } else {
                        System.out.println("Error: Marks must be between 0 and 100!");
                    }
                    break;
                    
                case 3:
                    System.out.print("Enter new name: ");
                    String bothName = scanner.nextLine().trim();
                    System.out.print("Enter new marks (0-100): ");
                    double bothMarks = scanner.nextDouble();
                    scanner.nextLine();
                    
                    if (!bothName.isEmpty() && bothMarks >= 0 && bothMarks <= 100) {
                        student.setName(bothName);
                        student.setMarks(bothMarks);
                        System.out.println("✓ Student details updated successfully!");
                    } else {
                        System.out.println("Error: Invalid input!");
                    }
                    break;
                    
                default:
                    System.out.println("Invalid choice!");
                    return;
            }
            
            System.out.println("Updated Details: " + student);
            
        } catch (InputMismatchException e) {
            System.out.println("Error: Please enter valid input!");
            scanner.nextLine(); // Clear invalid input
        }
    }
    
    // DELETE: Delete a student
    private static void deleteStudent() {
        System.out.println("\n--- DELETE STUDENT ---");
        
        if (students.isEmpty()) {
            System.out.println("No students found in the system.");
            return;
        }
        
        System.out.print("Enter student ID to delete: ");
        try {
            int id = scanner.nextInt();
            scanner.nextLine(); // Consume newline
            
            Student student = findStudentById(id);
            if (student == null) {
                System.out.println("Error: Student with ID " + id + " not found!");
                return;
            }
            
            System.out.println("Student to delete: " + student);
            System.out.print("Are you sure you want to delete this student? (y/n): ");
            String confirm = scanner.nextLine().trim().toLowerCase();
            
            if (confirm.equals("y") || confirm.equals("yes")) {
                students.remove(student);
                System.out.println("✓ Student deleted successfully!");
            } else {
                System.out.println("Delete operation cancelled.");
            }
            
        } catch (InputMismatchException e) {
            System.out.println("Error: Please enter valid student ID!");
            scanner.nextLine(); // Clear invalid input
        }
    }
    
    // SEARCH: Search for a student
    private static void searchStudent() {
        System.out.println("\n--- SEARCH STUDENT ---");
        
        if (students.isEmpty()) {
            System.out.println("No students found in the system.");
            return;
        }
        
        System.out.println("Search by:");
        System.out.println("1. ID");
        System.out.println("2. Name");
        System.out.print("Enter choice (1-2): ");
        
        try {
            int searchChoice = scanner.nextInt();
            scanner.nextLine(); // Consume newline
            
            switch (searchChoice) {
                case 1:
                    System.out.print("Enter student ID: ");
                    int id = scanner.nextInt();
                    scanner.nextLine();
                    
                    Student student = findStudentById(id);
                    if (student != null) {
                        System.out.println("Student found:");
                        System.out.println(student);
                    } else {
                        System.out.println("No student found with ID: " + id);
                    }
                    break;
                    
                case 2:
                    System.out.print("Enter student name (partial match allowed): ");
                    String name = scanner.nextLine().trim().toLowerCase();
                    
                    ArrayList<Student> foundStudents = new ArrayList<>();
                    for (Student s : students) {
                        if (s.getName().toLowerCase().contains(name)) {
                            foundStudents.add(s);
                        }
                    }
                    
                    if (foundStudents.isEmpty()) {
                        System.out.println("No students found with name containing: " + name);
                    } else {
                        System.out.println("Found " + foundStudents.size() + " student(s):");
                        System.out.println("-".repeat(55));
                        for (Student s : foundStudents) {
                            System.out.println(s);
                        }
                        System.out.println("-".repeat(55));
                    }
                    break;
                    
                default:
                    System.out.println("Invalid choice!");
            }
            
        } catch (InputMismatchException e) {
            System.out.println("Error: Please enter valid input!");
            scanner.nextLine(); // Clear invalid input
        }
    }
    
    // Helper method to find student by ID
    private static Student findStudentById(int id) {
        for (Student student : students) {
            if (student.getId() == id) {
                return student;
            }
        }
        return null;
    }
}

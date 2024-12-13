# Student-Result-Management-System
//Java project

import java.util.ArrayList;
import java.util.Scanner;

class Student {
    private int id;
    private String name;
    private int mathMarks;
    private int physicsMarks;
    private int ictMarks;
    private int totalMarks;
    private double average;
    private String grade;
    private double cgpa;

    public Student(int id, String name, int mathMarks, int physicsMarks, int ictMarks) {
        this.id = id;
        this.name = name;
        this.mathMarks = mathMarks;
        this.physicsMarks = physicsMarks;
        this.ictMarks = ictMarks;
        calculateResults();
    }

    private void calculateResults() {
        this.totalMarks = mathMarks + physicsMarks + ictMarks;
        this.average = totalMarks / 3.0;
        calculateGradeAndCGPA();
    }

    private void calculateGradeAndCGPA() {
        cgpa = (getGradePoint(mathMarks) + getGradePoint(physicsMarks) + getGradePoint(ictMarks)) / 3.0;

        if (mathMarks < 40 || physicsMarks < 40 || ictMarks < 40) {
            grade = "F";
        } else if (cgpa == 4.0) {
            grade = "A+";
        } else if (cgpa >= 3.75) {
            grade = "A";
        } else if (cgpa >= 3.5) {
            grade = "B+";
        } else if (cgpa >= 3.0) {
            grade = "B";
        } else if (cgpa >= 2.5) {
            grade = "C";
        } else {
            grade = "D";
        }
    }

    private double getGradePoint(int marks) {
        if (marks >= 80) return 4.0;
        if (marks >= 75) return 3.75;
        if (marks >= 70) return 3.5;
        if (marks >= 65) return 3.25;
        if (marks >= 60) return 3.0;
        if (marks >= 55) return 2.75;
        if (marks >= 50) return 2.5;
        if (marks >= 45) return 2.25;
        if (marks >= 40) return 2.0;
        return 0.0;
    }

    public void displayDetails() {
        System.out.printf("ID: %d\n", id);
        System.out.printf("Name: %s\n", name);
        System.out.printf("Math Marks: %d\n", mathMarks);
        System.out.printf("Physics Marks: %d\n", physicsMarks);
        System.out.printf("ICT Marks: %d\n", ictMarks);
        System.out.printf("Total Marks: %d\n", totalMarks);
        System.out.printf("Average Marks: %.2f\n", average);
        System.out.printf("CGPA: %.2f\n", cgpa);
        System.out.printf("Grade: %s\n", grade);
    }

    public int getId() {
        return id;
    }
}

public class StudentResultManagement {
    private static final Scanner scanner = new Scanner(System.in);
    private static final ArrayList<Student> students = new ArrayList<>();
    private static final String ADMIN_PASSWORD = "admin123";

    public static void main(String[] args) {
        int choice;

        do {
            System.out.println("\nSTUDENT RESULT MANAGEMENT SYSTEM");
            System.out.println("1. Add Student Information");
            System.out.println("2. Show Student Results");
            System.out.println("3. Exit");
            System.out.print("Enter your choice: ");
            choice = scanner.nextInt();

            switch (choice) {
                case 1:
                    if (authenticateAdmin()) {
                        addStudentInfo();
                    } else {
                        System.out.println("Incorrect password. Access denied.");
                    }
                    break;
                case 2:
                    showStudentResults();
                    break;
                case 3:
                    System.out.println("Exiting... Goodbye!");
                    break;
                default:
                    System.out.println("Invalid choice! Please try again.");
            }
        } while (choice != 3);
    }

    private static boolean authenticateAdmin() {
        System.out.print("Enter admin password: ");
        String inputPassword = scanner.next();
        return ADMIN_PASSWORD.equals(inputPassword);
    }

    private static void addStudentInfo() {
        System.out.print("Enter Student ID: ");
        int id = scanner.nextInt();
        scanner.nextLine(); // Consume the newline
        System.out.print("Enter Student Name: ");
        String name = scanner.nextLine();
        System.out.print("Enter Math Marks: ");
        int mathMarks = scanner.nextInt();
        System.out.print("Enter Physics Marks: ");
        int physicsMarks = scanner.nextInt();
        System.out.print("Enter ICT Marks: ");
        int ictMarks = scanner.nextInt();

        students.add(new Student(id, name, mathMarks, physicsMarks, ictMarks));
        System.out.println("Student information added successfully!");
    }

    private static void showStudentResults() {
        if (students.isEmpty()) {
            System.out.println("No student information available.");
            return;
        }

        System.out.print("Enter Student ID to view details: ");
        int id = scanner.nextInt();
        boolean found = false;

        for (Student student : students) {
            if (student.getId() == id) {
                student.displayDetails();
                found = true;
                break;
            }
        }

        if (!found) {
            System.out.println("Student with ID " + id + " not found.");
        }
    }
}

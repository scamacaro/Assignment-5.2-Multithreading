# Assignment-5.2-Multithreading
/*
 Sanyerlis Camacaro - Sanyerliscamacaro@uat.edu - CSC263
Assignment 5.2: Multithreading

Overview:
This assignment will help you learn to implement threads.

Guidelines:
Using the Runnable interface, design a Java program to demonstrate Multithreading.

About Code:
This Java program serves as an interactive multithreading demonstration. Upon initiation, 
the user is prompted to input their name, after which a brief welcome message is displayed. 
The program then enters a looped menu, offering three options: the first launches a 
predefined multithreading example with two threads concurrently counting from 1 to 5, 
the second allows customization of the example by specifying the number of threads and count 
limit, and the third option exits the program. 

The user interacts with the program by selecting a corresponding numerical option. The code utilizes 
the `Runnable` interface to define a class for the threads, and a `Scanner` for user input. 
 
The program continues to loop through the menu until the user opts to exit, ensuring an interactive 
and educational experience in exploring multithreading concepts in Java.
 */
// Import the Scanner class from the java.util package to facilitate user input
import java.util.Scanner;

// Define a class that implements the Runnable interface for multithreading
class MyRunnable implements Runnable {
    private String threadName;

    // Constructor to initialize the thread name
    MyRunnable(String name) {
        this.threadName = name;
    }

    // The run method that will be executed when the thread starts
    public void run() {
        for (int i = 1; i <= 5; i++) {
            // Print the thread name and the current count
            System.out.println(threadName + ": " + i);
            try {
                // Sleep for a short duration to simulate some work
                Thread.sleep(500);
            } catch (InterruptedException e) {
                // Print the stack trace if the thread is interrupted
                e.printStackTrace();
            }
        }
    }
}

// The main class that contains the program logic
public class Main {
    public static void main(String[] args) {
        // Print a welcome message
        System.out.println("\nWelcome to the Multithreading Demo!");

        // Ask the user for their name
        Scanner scanner = new Scanner(System.in);
        System.out.print("\nPlease Enter your name: ");
        String userName = scanner.nextLine();

        // Greet the user
        System.out.println("Hello, " + userName + "! Let's explore multithreading in action.");

        // Flag to control program exit
        boolean exitProgram = false;

        // Main program loop
        while (!exitProgram) {
            // Explain the program to the user
            System.out.println("\nThis program demonstrates multithreading using two threads.");
            System.out.println("Each thread counts from 1 to 5 with a short delay between each count.");

            // Provide options for the user
            System.out.println("\nChoose an option:");
            System.out.println("1. Run the multithreading example");
            System.out.println("2. Customize multithreading example");
            System.out.println("3. Exit");

            // Get user input for the selected option
            int option = scanner.nextInt();

            // Process the user's choice
            if (option == 1) {
                // Create two instances of MyRunnable
                MyRunnable myRunnable1 = new MyRunnable("Thread 1");
                MyRunnable myRunnable2 = new MyRunnable("Thread 2");

                // Create two threads and associate them with MyRunnable instances
                Thread thread1 = new Thread(myRunnable1);
                Thread thread2 = new Thread(myRunnable2);

                // Start the threads
                thread1.start();
                thread2.start();

                // Wait for both threads to finish before going back to the menu
                try {
                    thread1.join();
                    thread2.join();
                } catch (InterruptedException e) {
                    // Print the stack trace if the thread is interrupted during join
                    e.printStackTrace();
                }
            } else if (option == 2) {
                // Customize multithreading example
                System.out.println("\nCustomize Multithreading Example:");

                // Get the number of threads from the user
                System.out.print("Enter the number of threads (1 or 2): ");
                int numThreads = scanner.nextInt();

                // Get the count limit from the user
                System.out.print("Enter the count limit for each thread: ");
                int countLimit = scanner.nextInt();

                // Loop to create and start the specified number of threads
                for (int i = 1; i <= numThreads; i++) {
                    // Create instances of MyRunnable based on the number of threads
                    MyRunnable myRunnable = new MyRunnable("Thread " + i);

                    // Create threads and associate them with MyRunnable instances
                    Thread thread = new Thread(myRunnable);

                    // Start the threads
                    thread.start();

                    // Wait for the threads to finish before going back to the menu
                    try {
                        thread.join();
                    } catch (InterruptedException e) {
                        // Print the stack trace if the thread is interrupted during join
                        e.printStackTrace();
                    }
                }
            } else if (option == 3) {
                // Exit the program
                System.out.println("Exiting the program. Goodbye!");
                exitProgram = true;
            } else {
                // Handle invalid user input
                System.out.println("Invalid option. Please choose a valid option.");
            }
        }

        // Close the scanner
        scanner.close();
    }
}


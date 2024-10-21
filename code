#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <ctype.h> // For character checking functions

#define TABLE_SIZE 10 // Size of the hash table
#define NAME_LENGTH 50

// Define the structure for a student record
typedef struct Student {
    int id;
    char name[NAME_LENGTH];
    int isOccupied;       // Flag to indicate if the slot is occupied
} Student;

// Hash table
Student hashTable[TABLE_SIZE];

// Function prototypes
unsigned int hashFunction(int id);
void addStudent(int id, const char* name);
void displayStudents();
void searchStudent();
void searchByID(int id);
void searchByName(const char* name);
void initializeTable();
int isValidName(const char* name);
int isValidID(const char* input);

int main() {
    int choice;
    int id;
    char name[NAME_LENGTH];
    char idInput[NAME_LENGTH];

    // Initialize the hash table
    initializeTable();

    while (1) {
        printf("\nStudent Record System\n");
        printf("1. Add Student\n");
        printf("2. Display Students\n");
        printf("3. Search Student\n");
        printf("4. Exit\n");
        printf("Enter your choice: ");
        scanf("%d", &choice);

        switch (choice) {
            case 1:
                // Input student ID and validate it
                while (1) {
                    printf("Enter student ID (integer): ");
                    getchar(); // To consume newline character left by previous input
                    fgets(idInput, sizeof(idInput), stdin);
                    idInput[strcspn(idInput, "\n")] = 0; // Remove newline character

                    if (isValidID(idInput)) {
                        id = atoi(idInput); // Convert string to integer after validation
                        break; // Exit loop if ID is valid
                    } else {
                        printf("Error: Please enter a valid integer value for ID.\n");
                    }
                }

                // Input student name and validate it
                while (1) {
                    printf("Enter student name: ");
                    fgets(name, sizeof(name), stdin);
                    name[strcspn(name, "\n")] = 0; // Remove newline character

                    if (isValidName(name)) {
                        break; // Exit loop if name is valid
                    } else {
                        printf("Error: Name can only contain alphabetic characters. Please try again.\n");
                    }
                }

                addStudent(id, name);
                break;
            case 2:
                displayStudents();
                break;
            case 3:
                searchStudent();
                break;
            case 4:
                printf("Exiting...\n");
                exit(0);
            default:
                printf("Invalid choice. Please try again.\n");
        }
    }

    return 0;
}

// Hash function to compute index using integer ID
unsigned int hashFunction(int id) {
    return id % TABLE_SIZE;
}

// Function to initialize the hash table
void initializeTable() {
    for (int i = 0; i < TABLE_SIZE; ++i) {
        hashTable[i].isOccupied = 0; // Mark all slots as empty
    }
}

// Function to add a student record
void addStudent(int id, const char* name) {
    unsigned int index = hashFunction(id);
    unsigned int originalIndex = index;

    // Linear probing to handle collisions
    while (hashTable[index].isOccupied) {
        if (hashTable[index].id == id) {
            printf("Error: A student with ID %d already exists.\n", id);
            return;
        }
        index = (index + 1) % TABLE_SIZE;
        if (index == originalIndex) {
            printf("Error: Hash table is full, cannot add more students.\n");
            return;
        }
    }

    // Insert the student into the hash table
    hashTable[index].id = id;
    strcpy(hashTable[index].name, name);
    hashTable[index].isOccupied = 1;

    printf("Student added successfully.\n");
}

// Function to display all student records in a better format
void displayStudents() {
    printf("\n===========================\n");
    printf("  Student Records\n");
    printf("===========================\n");
    printf("%-10s %-30s\n", "ID", "Name"); // Header row with column titles
    printf("---------------------------\n");
    
    int empty = 1;
    for (int i = 0; i < TABLE_SIZE; ++i) {
        if (hashTable[i].isOccupied) {
            empty = 0;
            printf("%-10d %-30s\n", hashTable[i].id, hashTable[i].name); // Print ID and Name in a neat format
        }
    }
    
    if (empty) {
        printf("No student records found.\n");
    }
    
    printf("===========================\n");
}

// Function to search for a student record by ID or name
void searchStudent() {
    int searchChoice, id;
    char name[NAME_LENGTH];

    printf("Search by: \n");
    printf("1. Student ID\n");
    printf("2. Student Name\n");
    printf("Enter your choice: ");
    scanf("%d", &searchChoice);

    switch (searchChoice) {
        case 1:
            printf("Enter student ID: ");
            scanf("%d", &id);
            searchByID(id);
            break;
        case 2:
            printf("Enter student name: ");
            getchar(); // To consume newline character
            fgets(name, sizeof(name), stdin);
            name[strcspn(name, "\n")] = 0; // Remove newline character
            searchByName(name);
            break;
        default:
            printf("Invalid choice.\n");
    }
}

// Function to search student by ID
void searchByID(int id) {
    unsigned int index = hashFunction(id);
    unsigned int originalIndex = index;

    // Linear probing to handle collisions
    while (hashTable[index].isOccupied) {
        if (hashTable[index].id == id) {
            printf("Student found:\n");
            printf("ID: %d\n", hashTable[index].id);
            printf("Name: %s\n", hashTable[index].name);
            return;
        }
        index = (index + 1) % TABLE_SIZE;
        if (index == originalIndex) {
            break;
        }
    }

    printf("Error: Student with ID %d not found.\n", id);
}

// Function to search student by name
void searchByName(const char* name) {
    for (int i = 0; i < TABLE_SIZE; ++i) {
        if (hashTable[i].isOccupied && strcmp(hashTable[i].name, name) == 0) {
            printf("Student found:\n");
            printf("ID: %d\n", hashTable[i].id);
            printf("Name: %s\n", hashTable[i].name);
            return;
        }
    }

    printf("Error: Student with name '%s' not found.\n", name);
}

// Function to check if a name contains only alphabetic characters
int isValidName(const char* name) {
    for (int i = 0; name[i] != '\0'; i++) {
        if (!isalpha(name[i]) && name[i] != ' ') { // Allow spaces between words
            return 0; // Invalid character found
        }
    }
    return 1; // All characters are valid
}

// Function to check if the input is a valid integer
int isValidID(const char* input) {
    for (int i = 0; input[i] != '\0'; i++) {
        if (!isdigit(input[i])) {
            return 0; // If any character is not a digit, it's an invalid ID
        }
    }
    return 1; // All characters are digits, so it's a valid ID
}

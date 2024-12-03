#include <stdio.h>
#include <string.h>
#include <stdbool.h>
#include <ctype.h>

#define MAX_CONTACTS 100
#define NAME_SIZE 50
#define EMAIL_SIZE 50

// Define a structure for a contact
struct Contact {
    char firstName[NAME_SIZE];
    char lastName[NAME_SIZE];
    char email[EMAIL_SIZE];
};

// Array to store contacts
struct Contact contacts[MAX_CONTACTS];
int contactCount = 0;

// Function to validate email format (basic check)
bool isValidEmail(const char *email) {
    const char *at = strchr(email, '@');
    const char *dot = strrchr(email, '.');
    return at && dot && at < dot;
}

// Function to add a new contact
void addContact() {
    if (contactCount >= MAX_CONTACTS) {
        printf("Contact list is full!\n");
        return;
    }

    struct Contact newContact;

    // Input first name
    printf("Enter first name: ");
    fgets(newContact.firstName, sizeof(newContact.firstName), stdin);
    newContact.firstName[strcspn(newContact.firstName, "\n")] = '\0';  // Remove newline character

    // Input last name
    printf("Enter last name: ");
    fgets(newContact.lastName, sizeof(newContact.lastName), stdin);
    newContact.lastName[strcspn(newContact.lastName, "\n")] = '\0';  // Remove newline character

    // Input email and check for duplicates
    printf("Enter email: ");
    fgets(newContact.email, sizeof(newContact.email), stdin);
    newContact.email[strcspn(newContact.email, "\n")] = '\0';  // Remove newline character

    // Validate email format
    if (!isValidEmail(newContact.email)) {
        printf("Invalid email format!\n");
        return;
    }

    // Check if the email is already in use (case insensitive)
    for (int i = 0; i < contactCount; i++) {
        if (strcasecmp(contacts[i].email, newContact.email) == 0) {
            printf("A contact with this email already exists!\n");
            return;
        }
    }

    // Add the new contact to the array
    contacts[contactCount] = newContact;
    contactCount++;
    printf("Contact added successfully!\n");
}

// Function to display all contacts
void displayContacts() {
    if (contactCount == 0) {
        printf("No contacts to display.\n");
        return;
    }

    printf("\nList of Contacts:\n");
    for (int i = 0; i < contactCount; i++) {
        printf("Contact %d: %s %s, Email: %s\n", i + 1, contacts[i].firstName, contacts[i].lastName, contacts[i].email);
    }
}

// Function to search for a contact by email
void searchContactByEmail() {
    char email[EMAIL_SIZE];

    // Input email to search
    printf("Enter email to search: ");
    fgets(email, sizeof(email), stdin);
    email[strcspn(email, "\n")] = '\0';  // Remove newline character

    // Search for the contact by email (case insensitive)
    for (int i = 0; i < contactCount; i++) {
        if (strcasecmp(contacts[i].email, email) == 0) {
            printf("Contact found: %s %s, Email: %s\n", contacts[i].firstName, contacts[i].lastName, contacts[i].email);
            return;
        }
    }

    printf("No contact found with the provided email.\n");
}

// Function to delete a contact by email
void deleteContactByEmail() {
    char email[EMAIL_SIZE];

    // Input email to delete
    printf("Enter email of the contact to delete: ");
    fgets(email, sizeof(email), stdin);
    email[strcspn(email, "\n")] = '\0';  // Remove newline character

    for (int i = 0; i < contactCount; i++) {
        if (strcasecmp(contacts[i].email, email) == 0) {
            // Shift remaining contacts left
            for (int j = i; j < contactCount - 1; j++) {
                contacts[j] = contacts[j + 1];
            }
            contactCount--;
            printf("Contact deleted successfully!\n");
            return;
        }
    }

    printf("No contact found with the provided email.\n");
}

int main() {
    int choice;

    do {
        // Menu options
        printf("\nContact Information Management System\n");
        printf("1. Add Contact\n");
        printf("2. Display All Contacts\n");
        printf("3. Search Contact by Email\n");
        printf("4. Delete Contact by Email\n");
        printf("5. Exit\n");
        printf("Enter your choice: ");
        
        scanf("%d", &choice);
        getchar();  // Clear the newline character from the input buffer

        switch (choice) {
            case 1:
                addContact();
                break;
            case 2:
                displayContacts();
                break;
            case 3:
                searchContactByEmail();
                break;
            case 4:
                deleteContactByEmail();
                break;
            case 5:
                printf("Exiting the program...\n");
                break;
            default:
                printf("Invalid choice! Please try again.\n");
        }
        
    } while (choice != 5);

    return 0;
}

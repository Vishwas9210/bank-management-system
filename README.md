#include <iostream>
#include <fstream>
using namespace std;

// Account structure
struct Account {
    int accountNumber;
    char name[100];
    float balance;
    char address[100];
    char phoneNumber[50];
    char email[100];
};

// Function prototypes
void createAccount();
void deposit();
void withdraw();
void checkBalance();
void displayAllAccounts();
void showMenu();

int main() {
    showMenu();
    return 0;
}

void showMenu() {
    int choice;
    do {
        cout << "\n--- Bank Management System ---\n";
        cout << "1. Create Account\n";
        cout << "2. Deposit\n";
        cout << "3. Withdraw\n";
        cout << "4. Check Balance\n";
        cout << "5. Display All Accounts\n";
        cout << "6. Exit\n";
        cout << "Enter your choice: ";
        cin >> choice;

        switch (choice) {
            case 1: createAccount(); break;
            case 2: deposit(); break;
            case 3: withdraw(); break;
            case 4: checkBalance(); break;
            case 5: displayAllAccounts(); break;
            case 6: cout << "Exiting...\n thankyou for your time"; break;
            default: cout << "Invalid choice. Please try again.\n";
        }
    } while (choice != 6);
}

void createAccount() {
    Account account;
    ofstream outFile;

    cout << "\n--- Create Account ---\n";
    cout << "Enter Account Number: ";
    cin >> account.accountNumber;
    cin.ignore();  // To ignore the newline character left in the buffer

    cout << "Enter Name: ";
    cin.getline(account.name, 100);

    cout << "Enter Address: ";
    cin.getline(account.address, 100);

    cout << "Enter Phone Number: ";
    cin.getline(account.phoneNumber,50);

    cout << "Enter Email: ";
    cin.getline(account.email, 100);

    account.balance = 0.0;

    outFile.open("accounts.txt", ios::app);
    if (outFile) {
        outFile << account.accountNumber << " " << account.name << " " << account.balance << " "
                << account.address << " " << account.phoneNumber << " " << account.email << endl;
        outFile.close();
        cout << "\t\t\tAccount created successfully!\n";
    } else {
        cout << "\t\t\tError creating account.\n";
    }
}

void deposit() {
    int accNum;
    float amount;
    Account account;
    ifstream inFile;
    ofstream outFile;

    cout << "\n--- Deposit ---\n";
    cout << "Enter Account Number: ";
    cin >> accNum;
    cout << "Enter Amount to Deposit: ";
    cin >> amount;

    inFile.open("accounts.txt");
    outFile.open("temp.txt");

    bool found = false;
    while (inFile >> account.accountNumber >> account.name >> account.balance >> account.address >> account.phoneNumber >> account.email) {
        if (account.accountNumber == accNum) {
            account.balance += amount;
            found = true;
            cout << "\t\t\tAmount deposited successfully!\n";
        }
        outFile << account.accountNumber << " " << account.name << " " << account.balance << " "
                << account.address << " " << account.phoneNumber << " " << account.email << endl;
    }

    inFile.close();
    outFile.close();

    if (found) {
        remove("accounts.txt");
        rename("temp.txt", "accounts.txt");
    } else {
        cout << "Account not found.\n";
        remove("temp.txt");
    }
}

void withdraw() {
    int accNum;
    float amount;
    Account account;
    ifstream inFile;
    ofstream outFile;

    cout << "\n--- Withdraw ---\n";
    cout << "Enter Account Number: ";
    cin >> accNum;
    cout << "Enter Amount to Withdraw: ";
    cin >> amount;

    inFile.open("accounts.txt");
    outFile.open("temp.txt");

    bool found = false;
    while (inFile >> account.accountNumber >> account.name >> account.balance >> account.address >> account.phoneNumber >> account.email) {
        if (account.accountNumber == accNum) {
            if (account.balance >= amount) {
                account.balance -= amount;
                found = true;
                cout << "\t\t\tAmount withdrawn successfully!\n";
            } else {
                cout << "Insufficient balance.\n";
            }
        }
        outFile << account.accountNumber << " " << account.name << " " << account.balance << " "
                << account.address << " " << account.phoneNumber << " " << account.email << endl;
    }

    inFile.close();
    outFile.close();

    if (found) {
        remove("accounts.txt");
        rename("temp.txt", "accounts.txt");
    } else {
        cout << "Account not found.\n";
        remove("temp.txt");
    }
}

void checkBalance() {
    int accNum;
    Account account;
    ifstream inFile;

    cout << "\n--- Check Balance ---\n";
    cout << "Enter Account Number: ";
    cin >> accNum;

    inFile.open("accounts.txt");

    bool found = false;
    while (inFile >> account.accountNumber >> account.name >> account.balance >> account.address >> account.phoneNumber >> account.email) {
        if (account.accountNumber == accNum) {
            cout << "Account Number: " << account.accountNumber << endl;
            cout << "Name: " << account.name << endl;
            cout << "Balance: " << account.balance << endl;
            cout << "Address: " << account.address << endl;
            cout << "Phone Number: " << account.phoneNumber << endl;
            cout << "Email: " << account.email << endl;
            found = true;
            break;
        }
    }

    if (!found) {
        cout << "Account not found.\n";
    }

    inFile.close();
}

void displayAllAccounts() {
    Account account;
    ifstream inFile;

    cout << "\n--- All Accounts ---\n";
    inFile.open("accounts.txt");

    while (inFile >> account.accountNumber >> account.name >> account.balance >> account.address >> account.phoneNumber >> account.email) {
        cout << "Account Number: " << account.accountNumber << endl;
        cout << "Name: " << account.name << endl;
        cout << "Balance: " << account.balance << endl;
        cout << "Address: " << account.address << endl;
        cout << "Phone Number: " << account.phoneNumber << endl;
        cout << "Email: " << account.email << endl;
        cout << "-------------------------\n";
    }

    inFile.close();
}

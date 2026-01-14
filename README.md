// Clinic Appointment Booking System 

#include <iostream>
#include <fstream>
#include <string>
#include <iomanip> //  use setw () for table format 

using namespace std;

// ===== Constant =====
const int MAX = 100;
const string FILE_NAME = "appointments.txt";

// ===== Structure =====
struct Appointment {
    int id;
    string name;
    string phone;
    string date;
    string time;
    int status; // 1 = Active, 0 = Cancelled
};

// ===== Global Variables =====
Appointment appts[MAX];
int countAppt = 0;
int nextID = 1;

// ===== Function Prototypes =====
void showMenu();
void addAppointment();
void displayAppointments();
void searchAppointment();
void editAppointment(); 
void cancelAppointment();
void saveToFile();
void loadFromFile();
int findIndexById(int id);



// ===== Menu =====
void showMenu() {
    cout << "\n=== Clinic Appointment System ===\n";
    cout << "1. Add Appointment\n";
    cout << "2. Display Appointments\n";
    cout << "3. Search Appointment\n";
    cout << "4. Edit Appointment\n"; 
    cout << "5. Cancel Appointment\n";
    cout << "6. Save to File\n";
    cout << "7. Load from File\n";
    cout << "0. Exit\n";
    cout << "Enter choice: ";
}

// ===== Display Appointments =====
void displayAppointments() {
    if (countAppt == 0) {
        cout << "No appointments found.\n";
        return;
    }

    // Creating  header 
    cout << left << setw(5) << "ID"
        << setw(20) << "Name"
        << setw(15) << "Phone"
        << setw(12) << "Date"
        << setw(8) << "Time"
        << "Status" << endl;
    cout << string(70, '-') << endl;

    for (int i = 0; i < countAppt; i++) {
        string statusStr = (appts[i].status == 1) ? "Active" : "Cancelled";

        cout << left << setw(5) << appts[i].id
            << setw(20) << appts[i].name
            << setw(15) << appts[i].phone
            << setw(12) << appts[i].date
            << setw(8) << appts[i].time
            << statusStr << endl;
    }
}

// ===== Search Appointment =====
void searchAppointment() {
    string keyword;
    bool found = false;

    cin.ignore();
    cout << "Enter patient name to search: ";
    getline(cin, keyword);

    cout << "\n--- Search Results ---\n";
    for (int i = 0; i < countAppt; i++) {
        // string::npos check
        if (appts[i].name.find(keyword) != string::npos) {
            cout << "ID: " << appts[i].id
                << " | Name: " << appts[i].name
                << " | Status: " << (appts[i].status == 1 ? "Active" : "Cancelled") << endl;
            found = true;
        }
    }

    if (!found) {
        cout << "No matching records found.\n";
    }
}

// ===== Save to File =====
void saveToFile() {
    ofstream file(FILE_NAME.c_str());

    if (!file) {
        cout << "Error opening file for write.\n";
        return;
    }

    for (int i = 0; i < countAppt; i++) {
        file << appts[i].id << "|"
            << appts[i].name << "|"
            << appts[i].phone << "|"
            << appts[i].date << "|"
            << appts[i].time << "|"
            << appts[i].status << endl;
    }

    file.close();
    cout << "Data saved successfully.\n";
}
// ===== Load from File =====
void loadFromFile() {
    ifstream file(FILE_NAME.c_str());
    countAppt = 0;

    if (!file) {
        
        return;
    }

    // === Check countAppt < MAX to prevent crash ===
    while (countAppt < MAX && file >> appts[countAppt].id) {
        file.ignore(); // skip |
        getline(file, appts[countAppt].name, '|');
        getline(file, appts[countAppt].phone, '|');
        getline(file, appts[countAppt].date, '|');
        getline(file, appts[countAppt].time, '|');
        file >> appts[countAppt].status;
        file.ignore(); // skip newline

        // Sync nextID so it doesn't duplicate IDs after loading
        if (appts[countAppt].id >= nextID)
            nextID = appts[countAppt].id + 1;

        countAppt++;
    }

    file.close();
    cout << "Loaded " << countAppt << " appointments.\n";
}

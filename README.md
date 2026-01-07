# Clinic-Appointment-Booking-System
//Clinic Appointment Booking System
#include <iostream>
#include <fstream>
#include <sstream>
#include <string>
#include <limits>

using namespace std;

const int MAX_APPTS = 500;
const string DATA_FILE = "appoinments.txt";

struct Appointment {
	int id ;			
	string patientName ;
	string phone ;
	string date ;   // YYYY-MM-DD
	string time ;   // HH:MM
	string doctor ;
	string notes ;
	bool active ;
};

// === Global Data ===
Appointment appts[MAX_APPTS];
int apptCount = 0;
int nextID = 1;

//utility; trim helpers
string trim(const string& s){
size_t start =
s.find_first_not_of("\t\r\n");
if (start == string::npos)return "";
size_t end = s.find_last_not_of("\t\r\n");
return s.substr(start, end - start + 1);
}

// load appointments from file (chelsea)
void loadData(){
ifstream file(FILE_NAME);

if(!file){
cout << "Starting...\n";
return;
}

apptCount = 0;

while (file >> appointments[apptCount].id){
file.ignore();
getline(file, appointments[apptCount].patientName);
        getline(file, appointments[apptCount].phone);
        getline(file, appointments[apptCount].date);
        getline(file, appointments[appotCount].time);
        getline(file, appointments[apptCount].doctor);
        getline(file, appointments[apptCount].status);
        getline(file, appointments[apptCount].notes);
        
        apptCount++;
        
        if (appotCount >= MAX_APPTS) break;
    }
    
    file.close();
    cout << "Loaded " << apptCount << " appointments.\n";
}

//Ung Pun Kang
// Save appointments to file
void saveToFile()
{
    ofstream outfile("DATA_FILE");

    if (!outfile)
    {
        cout << "Error opening file.\n";
        return;
    }

    for (int i = 0; i < apptCount; i++)
    {
        outfile << appts[i].id << "|"
            << appts[i].patientName << "|"
            << appts[i].phone << "|"
            << appts[i].date << "|"
            << appts[i].time << "|"
            << appts[i].doctor << "|"
            << appts[i].notes << "|";

        if (appts[i].active)
            outfile << "1\n";
        else
            outfile << "0\n";
    }

    outfile.close();
    return 0;
}

//Nuhaa
// Display one appointment
void displayAppointment(const Appointment& a) {
    cout << "ID: " << a.id << (a.active ? " (Active)\n" : " (Cancelled)\n");
    cout << " Patient: " << a.patientName << "\n";
    cout << " Phone:   " << a.phone << "\n";
    cout << " Date:    " << a.date << "\n";
    cout << " Time:    " << a.time << "\n";
    cout << " Doctor:  " << a.doctor << "\n";
    cout << " Notes:   " << a.notes << "\n";
    cout << "-----------------------------\n";
}

//Nuhaa
// List all appointments
void listAppointments(bool showAll = true) {
    if (apptCount == 0) {
        cout << "No appointments.\n";
        return;
    }
    for (int i = 0; i < apptCount; ++i) {
        if (!showAll && !appts[i].active) continue;
        displayAppointment(appts[i]);
    }
}

// Yixin
// Add appointment
void addAppointment() {
    if (apptCount >= MAX_APPTS) {
        cout << "Appointment list is full.\n";
        return;
    }

    Appointment newAppt;
    newAppt.id = nextID++;
    newAppt.active = true;

    cin.ignore(numeric_limits<streamsize>::max(), '\n');

    cout << "Enter patient name: ";
    getline(cin, newAppt.patientName);

    cout << "Enter phone number: ";
    getline(cin, newAppt.phone);

    cout << "Enter appointment date (YYYY-MM-DD): ";
    getline(cin, newAppt.date);

    cout << "Enter appointment time (HH:MM): ";
    getline(cin, newAppt.time);

    cout << "Enter doctor name: ";
    getline(cin, newAppt.doctor);

    cout << "Enter notes (optional): ";
    getline(cin, newAppt.notes);

    appts[apptCount++] = newAppt;

    cout << "Appointment added successfully. ID: "
        << newAppt.id << endl;
}
//find index by id (chelsea)
int findAppointmentById(int id) {
    // Loop through all appointments
    for (int i = 0; i < apptCount; i++) {
        if (appts[i].id == id) {
            return i;  // Found it!
        }
    }
    return -1;  // Not found
}

// Yixin
// Search appointment by Patient Name
// Search Appointment by Patient Name
void searchAppointmentByName() {
    cin.ignore(numeric_limits<streamsize>::max(), '\n');

    string keyword;
    cout << "Enter patient name to search: ";
    getline(cin, keyword);

    bool found = false;

    for (int i = 0; i < apptCount; i++) {
        if (appts[i].patientName.find(keyword) != string::npos) {
            displayAppointment(appts[i]);
            found = true;
        }
    }

    if (!found) {
        cout << "No appointment found.\n";
    }
}

//edit appointment (chelsea)
void editAppointment() {
    int id;
    cout << "\nEnter appointment ID to edit: ";
    cin >> id;
    
    int index = findAppointmentById(id);
    
    if (index == -1) {
        cout << "Appointment not found!\n";
        return;
    }
	showAppointment(index);
    
    clearInput();
    cout << "\n=== EDIT APPOINTMENT ===\n";
    
    cout << "New patient name (current: " << appts[index].patientName << "): ";
    string input;
    getline(cin, input);
    if (!input.empty()) {
        appts[index].patientName = input;
    }
    
    cout << "New phone (current: " << appts[index].phone << "): ";
    getline(cin, input);
    if (!input.empty()) {
        appts[index].phone = input;
    }
    
    cout << "New date (current: " << appts[index].date << "): ";
    getline(cin, input);
    if (!input.empty()) {
        appts[index].date = input;
    }
    
    cout << "New time (current: " << appts[index].time << "): ";
    getline(cin, input);
    if (!input.empty()) {
        appts[index].time = input;
    }
    
    cout << "New doctor (current: " << appts[index].doctor << "): ";
    getline(cin, input);
    if (!input.empty()) {
        appts[index].doctor = input;
    }
    
    cout << "Appointment updated!\n";
}

// Yixin
// Cancel Appointment
void cancelAppointment() {
    int id;
    cout << "Enter appointment ID to cancel: ";
    cin >> id;

    bool found = false;

    for (int i = 0; i < apptCount; i++) {
        if (appts[i].id == id) {
            appts[i].active = false;
            cout << "Appointment cancelled successfully.\n";
            found = true;
            break;
        }
    }

    if (!found) {
        cout << "Appointment ID not found.\n";
    }
}

//Amalina
// Delete appointment (physically remove from array)
void deleteAppointment() {
    cout << "Enter appointment ID to delete permanently: ";
    int id; if (!(cin >> id)) { cin.clear(); cin.ignore(numeric_limits<streamsize>::max(), '\n'); cout << "Invalid input.\n"; return; }
    int idx = findByID(id);
    if (idx == -1) { cout << "Not found.\n"; return; }
    // shift left
    for (int i = idx; i < apptCount - 1; ++i) appts[i] = appts[i + 1];
    --apptCount;
    cout << "Appointment deleted.\n";
}

//Nuhaa
// Simple menu
void showMenu() {
    cout << "\n=== Clinic Appointment System ===\n";
    cout << "1. Add appointment\n";
    cout << "2. List all appointments\n";
    cout << "3. Search by patient name\n";
    cout << "4. Edit appointment\n";
    cout << "5. Cancel appointment (mark inactive)\n";
    cout << "6. Delete appointment (permanent)\n";
    cout << "7. Save to file\n";
    cout << "8. Load from file\n";
    cout << "9. List only active appointments\n";
    cout << "0. Exit\n";
    cout << "Select option: ";
}

//Amalina
int main() {
    loadFromFile();
    int choice;
    do {
        showMenu();
        if (!(cin >> choice)) {
            cin.clear();
            cin.ignore(numeric_limits<streamsize>::max(), '\n');
            cout << "Invalid option. Try again.\n";
            continue;
        }
        switch (choice) {
        case 1: addAppointment(); break;
        case 2: listAppointments(true); break;
        case 3: {
            cin.ignore(numeric_limits<streamsize>::max(), '\n');
            cout << "Enter name to search: ";
            string q; getline(cin, q);
            searchByName(q);
            break;
        }
        case 4: editAppointment(); break;
        case 5: cancelAppointment(); break;
        case 6: deleteAppointment(); break;
        case 7: saveToFile(); break;
        case 8: loadFromFile(); break;
        case 9: listAppointments(false); break;
        case 0: cout << "Exiting. Goodbye.\n"; break;
        default: cout << "Unknown option.\n"; break;
        }
    } while (choice != 0);

    // optional: auto-save on exit
    saveToFile();
    return 0;
}

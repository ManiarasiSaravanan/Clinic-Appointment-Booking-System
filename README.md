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
	int id = 0;
	string patientName = "";
	string phone = "";
	string date = "";   // YYYY-MM-DD
	string time = "";   // HH:MM
	string doctor = "";
	string notes = "";
	bool active = false;
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

// Yixin
//================= DISPLAY =================
void displayAppointment(const Appointment& a) {
    cout << "ID: " << a.id << (a.active ? " (Active)\n" : " (Cancelled)\n");
    cout << "Patient Name: " << a.patientName << endl;
    cout << "Phone: " << a.phone << endl;
    cout << "Date: " << a.date << endl;
    cout << "Time: " << a.time << endl;
    cout << "Doctor: " << a.doctor << endl;
    cout << "Notes: " << a.notes << endl;
    cout << "-----------------------------\n";

//Yixin 
// Add appoinment
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

// Yixin
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


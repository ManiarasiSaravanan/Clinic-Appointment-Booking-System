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


int main() {

    // Auto-save on exit 
    saveToFile();
    return 0;


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

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
	int id;
	string patientName;
	string phone;
	string date;// YYYY-MM-DD
	string time;//HH:MM
	string doctor;
	string notes;
	bool active;
};

//utility; trim helpers
string trim(const string& s){
size_t start =
s.find_first_not_of("\t\r\n");
if (start == string::npos)return "";
size_t end = s.find_last_not_of("\t\r\n");
return s.substr(start, end - start + 1);
}



#include <iostream>
#include <fstream>
#include <string>
using namespace std;

class Student {
private:
    int rollNo;
    string name;
    float marks;

public:
    void input() {
        cout << "Enter Roll Number: ";
        cin >> rollNo;
        cin.ignore();
        cout << "Enter Name: ";
        getline(cin, name);
        cout << "Enter Marks: ";
        cin >> marks;
    }

    void display() const {
        cout << "Roll No: " << rollNo << ", Name: " << name << ", Marks: " << marks << endl;
    }

    int getRollNo() const {
        return rollNo;
    }
};

void addStudent() {
    Student s;
    ofstream outFile("students.dat", ios::binary | ios::app);
    s.input();
    outFile.write(reinterpret_cast<char*>(&s), sizeof(s));
    outFile.close();
    cout << "Student record added.\n";
}

void displayStudents() {
    Student s;
    ifstream inFile("students.dat", ios::binary);
    if (!inFile) {
        cout << "No records found.\n";
        return;
    }
    while (inFile.read(reinterpret_cast<char*>(&s), sizeof(s))) {
        s.display();
    }
    inFile.close();
}

void searchStudent(int roll) {
    Student s;
    ifstream inFile("students.dat", ios::binary);
    bool found = false;
    while (inFile.read(reinterpret_cast<char*>(&s), sizeof(s))) {
        if (s.getRollNo() == roll) {
            s.display();
            found = true;
            break;
        }
    }
    inFile.close();
    if (!found)


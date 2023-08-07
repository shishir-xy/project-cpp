#include <iostream>
#include <string>
#include <vector>

using namespace std;

class Course
{
private:
    string name;
    string code;
    string instructor;
    vector<string> enrolledStudents;
    double price;

public:
    Course(string name1, string code1, string instructor1, double price1)
    {
        name = name1;
        code = code1;
        instructor = instructor1;
        price = price1;
    }

    string getName()
    {
        return name;
    }

    void enrollStudent(string studentName)
    {
        enrolledStudents.push_back(studentName);
    }

    vector<string> getEnrolledStudents() const { return enrolledStudents; }

    double getPrice() const { return price; }

    void displayDetails()
    {
        cout << "Course: " << name << " (" << code << ")\n";
        cout << "Instructor: " << instructor << "\n";
        cout << "Price: TK" << price << "\n";
        cout << "Enrolled Students: ";
        for (const auto &student : enrolledStudents)
        {
            cout << student << ", ";
        }
        cout << "\n";
    }
};

class Student
{
private:
    string name;
    string studentID;
    vector<string> enrolledCourses;

public:
    Student(string name, string studentID) : name(name), studentID(studentID) {}

    string getName() const { return name; }

    void enrollCourse(string courseName) { enrolledCourses.push_back(courseName); }

    vector<string> getEnrolledCourses() const { return enrolledCourses; }

    void displayDetails()
    {
        cout << "Student: " << name << " (" << studentID << ")\n";
        cout << "Enrolled Courses: ";
        for (const auto &course : enrolledCourses)
        {
            cout << course << ", ";
        }
        cout << "\n";
    }
};

class CourseManager
{
private:
    vector<Course> courses;
    vector<Student> students;

    vector<string> stuName;
    vector<string> stuId;

    int stuNo = 0;

    vector<string> course_name;
    vector<string> course_code;
    vector<string> course_instructor;
    vector<double> course_price;
    int courseNo = 0;

public:
    void addCourse(string name, string code, string instructor, double price)
    {
        courses.emplace_back(name, code, instructor, price);
        course_name.push_back(name);
        course_code.push_back(code);
        course_instructor.push_back(instructor);
        course_price.push_back(price);
        courseNo++;
    }

    void addStudent(string name, string studentID)
    {
        students.emplace_back(name, studentID);
        stuName.push_back(name);
        stuId.push_back(studentID);
        stuNo++;
    }

    void enrollStudentInCourse(string studentName, string courseName)
    {
        for (auto &course : courses)
        {
            if (course.getName() == courseName)
            {
                course.enrollStudent(studentName);
                break;
            }
        }
        for (auto &student : students)
        {
            if (student.getName() == studentName)
            {
                student.enrollCourse(courseName);
                break;
            }
        }
    }

    void displayAllCourses()
    {
        cout << "All Courses:\n";
        for (int i = 0; i < courseNo; i++)
        {
            cout << "course name : " << course_name[i] << endl;
            cout << "course code : " << course_code[i] << endl;
            cout << "course instructor : " << course_instructor[i] << endl;
            cout << "course price : " << course_price[i] << endl;
            cout << endl;
        }
    }
    void displayAllStudents()
    {
        cout << "All Students:\n";
        for (int i = 0; i < stuNo; i++)
        {
            cout << "Name is : " << stuName[i] << "\nID is : " << stuId[i] << endl;
        }
        cout << endl;
    }
    const vector<Course> &getCourses() const
    {
        return courses;
    }

    const vector<Student> &getStudents() const
    {
        return students;
    }
};

class Management
{
public:
    static int getTotalEnrolledStudents(const CourseManager &courseManager)
    {
        int totalStudents = 0;
        for (const auto &student : courseManager.getStudents())
        {
            totalStudents += student.getEnrolledCourses().size();
        }
        return totalStudents;
    }

    static double calculateTotalCourseFee(const CourseManager &courseManager)
    {
        double totalFee = 0;
        for (const auto &course : courseManager.getCourses())
        {
            totalFee += course.getPrice() * course.getEnrolledStudents().size();
        }
        return totalFee;
    }
};

int main()
{
    CourseManager courseManager;

    courseManager.addCourse("OOP", "CSE 1207 ", "Shahjalal Shohag", 1200.0);
    courseManager.addCourse("DLD", "CSE 1203", "Fahim Mia", 1000.0);

    courseManager.addStudent("Md. Mosaddek Ali", "2107027");
    courseManager.addStudent("Arman Rafi", "2107046");

    courseManager.enrollStudentInCourse("Md. Mosaddek Ali", "OOP");
    courseManager.enrollStudentInCourse("Md. Mosaddek Ali", "DLD");
    courseManager.enrollStudentInCourse("Arman Rafi", "OOP");

    courseManager.displayAllCourses();
    courseManager.displayAllStudents();

    int totalEnrolledStudents = Management::getTotalEnrolledStudents(courseManager);
    cout << "Total Enrolled Students: " << totalEnrolledStudents << endl;

    double totalCourseFee = Management::calculateTotalCourseFee(courseManager);
    cout << "Total Course Fee: TK " << totalCourseFee << endl;

    return 0;

}

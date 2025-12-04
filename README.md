# EEL_FYProject_mmcoe
#include <stdio.h>
#include <string.h>

#define MAX_STUDENTS 50
#define MAX_COURSES 50
#define MAX_ENROLL 200

// ----------------- DATA STRUCTURES -----------------
struct Student {
    int id;
    char name[50];
};

struct Course {
    int id;
    char title[50];
    char instructor[50];
};

struct Enrollment {
    int studentId;
    int courseId;
    float grade;
    int attendance;
};

// ----------------- GLOBAL ARRAYS -----------------
struct Student students[MAX_STUDENTS];
struct Course courses[MAX_COURSES];
struct Enrollment enrollments[MAX_ENROLL];

int studentCount = 0, courseCount = 0, enrollCount = 0;

// ----------------- FUNCTIONS -----------------

void addStudent() {
    struct Student s;
    printf("Enter Student ID: ");
    scanf("%d", &s.id);
    printf("Enter Student Name: ");
    scanf("%s", s.name);

    students[studentCount++] = s;
    printf("Student added successfully!\n");
}

void addCourse() {
    struct Course c;
    printf("Enter Course ID: ");
    scanf("%d", &c.id);
    printf("Enter Course Title: ");
    scanf("%s", c.title);
    printf("Enter Instructor Name: ");
    scanf("%s", c.instructor);

    courses[courseCount++] = c;
    printf("Course added successfully!\n");
}

void enrollStudent() {
    struct Enrollment e;
    int foundStudent = 0, foundCourse = 0;

    printf("Enter Student ID: ");
    scanf("%d", &e.studentId);

    // Validate student
    for (int i = 0; i < studentCount; i++) {
        if (students[i].id == e.studentId) {
            foundStudent = 1;
            break;
        }
    }

    if (!foundStudent) {
        printf("Student ID not found!\n");
        return;
    }

    printf("Enter Course ID: ");
    scanf("%d", &e.courseId);

    // Validate course
    for (int i = 0; i < courseCount; i++) {
        if (courses[i].id == e.courseId) {
            foundCourse = 1;
            break;
        }
    }

    if (!foundCourse) {
        printf("Course ID not found!\n");
        return;
    }

    e.grade = -1;
    e.attendance = 0;

    enrollments[enrollCount++] = e;
    printf("Student enrolled successfully!\n");
}

void enterGrade() {
    int sid, cid;
    float grade;

    printf("Enter Student ID: ");
    scanf("%d", &sid);
    printf("Enter Course ID: ");
    scanf("%d", &cid);

    for (int i = 0; i < enrollCount; i++) {
        if (enrollments[i].studentId == sid &&
            enrollments[i].courseId == cid) {

            printf("Enter Grade: ");
            scanf("%f", &grade);
            enrollments[i].grade = grade;
            printf("Grade updated!\n");
            return;
        }
    }

    printf("Enrollment not found!\n");
}

void markAttendance() {
    int sid, cid;
    printf("Enter Student ID: ");
    scanf("%d", &sid);
    printf("Enter Course ID: ");
    scanf("%d", &cid);

    for (int i = 0; i < enrollCount; i++) {
        if (enrollments[i].studentId == sid &&
            enrollments[i].courseId == cid) {

            enrollments[i].attendance += 1;
            printf("Attendance marked!\n");
            return;
        }
    }
    printf("Enrollment not found!\n");
}

void viewStudentSchedule() {
    int sid;
    printf("Enter Student ID: ");
    scanf("%d", &sid);

    printf("\nCourses for Student %d:\n", sid);
    for (int i = 0; i < enrollCount; i++) {
        if (enrollments[i].studentId == sid) {
            for (int j = 0; j < courseCount; j++) {
                if (courses[j].id == enrollments[i].courseId) {
                    printf("- %s (Instructor: %s)\n", courses[j].title, courses[j].instructor);
                }
            }
        }
    }
}

// ----------------- MAIN MENU -----------------

int main() {
    int choice;

    while (1) {
        printf("\n------ COURSE REGISTRATION SYSTEM ------\n");
        printf("1. Add Student\n");
        printf("2. Add Course\n");
        printf("3. Enroll Student in Course\n");
        printf("4. View Student Schedule\n");
        printf("5. Enter Grade\n");
        printf("6. Mark Attendance\n");
        printf("7. Exit\n");
        printf("Enter choice: ");
        scanf("%d", &choice);

        switch (choice) {
            case 1: addStudent(); break;
            case 2: addCourse(); break;
            case 3: enrollStudent(); break;
            case 4: viewStudentSchedule(); break;
            case 5: enterGrade(); break;
            case 6: markAttendance(); break;
            case 7: return 0;
            default: printf("Invalid choice!\n");
        }
    }

    return 0;
}

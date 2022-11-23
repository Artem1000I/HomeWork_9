# HomeWork_9
ДЗ


/*
1. Написать функцию, принимающую список Student и возвращающую список уникальных курсов, на которые подписаны студенты.
2. Написать функцию, принимающую на вход список Student и возвращающую список из трех самых любознательных (любознательность определяется количеством курсов).
3. Написать функцию, принимающую на вход список Student и экземпляр Course, возвращающую список студентов, которые посещают этот курс.
*/

package HomeWork_9;

public interface Course {

}


package HomeWork_9;

public class CourseImpl implements Course {
    private  String name;

    public CourseImpl(String name) {

        this.name = name;
    }

    @Override
    public String toString() {
        return name;
    }
}

package HomeWork_9;

import java.util.*;
import java.util.stream.Collectors;



public class Main {

    public static void main (String[] args) {
        Course course1 = new CourseImpl("SoapUI");
        Course course2 = new CourseImpl("Java Automation");
        Course course3 = new CourseImpl ("SQL");

        List<Student> students = Arrays.asList(
                new StudentImpl("Igor", Arrays.asList(course1, course2)),
                new StudentImpl("Sacha", Arrays.asList(course2, course3)),
                new StudentImpl("Violetta", Arrays.asList(course1, course3)),
                new StudentImpl("Seva", Arrays.asList(course1, course2, course3)),
                new StudentImpl("Kris", null)
        );

        System.out.println(getUniqueCourses(students));
        System.out.println(getInquisitiveStudent (students));
        System.out.println(getStudentsByCourses(students, course2));
    }

    public static List <Course> getUniqueCourses(List <Student> students) {
        students = students == null ? new ArrayList<>() :students ;

        return students.stream()
                .filter(Objects::nonNull)
                .map (Student::getAllCourses)
                .filter(Objects::nonNull)
                .flatMap(Collection::stream)
                .distinct()
                .collect(Collectors.toList());
    }

    public static List<Student> getInquisitiveStudent(List<Student> students) {
        students = students == null ? new ArrayList<>() : students;

        return students.stream()
                .filter(Objects::nonNull)
                .sorted((o1, o2) -> {
                    List<Course> c1 = o1.getAllCourses();
                    List<Course> c2 = o2.getAllCourses();
                    return Integer.compare(
                            c2 == null ? 0 : c2.size(),
                            c1 == null ? 0 : c1.size()
                    );
                })
                .limit (3)
                .collect(Collectors.toList());
    }

    public static List <Student> getStudentsByCourses(List <Student> students, Course course) {
        if (course == null) {
            return new ArrayList<>();
        }

        students = students == null ? new ArrayList<>() :students ;

        return students.stream()
                .filter(Objects::nonNull)
                .filter(student -> {
                    List<Course> courses = student.getAllCourses();
                    courses = courses == null ? Collections.emptyList() : courses;
                    return courses.contains(course);
                })
                .collect(Collectors.toList());
    }
}

package HomeWork_9;

import java.util.List;

public interface Student {
    String getName();

    List<Course> getAllCourses();

}


package HomeWork_9;

import java.util.ArrayList;
import java.util.List;

import java.util.List;

public class StudentImpl implements Student {
    private  String name;
    private List<Course> courses;

    public StudentImpl(String name, List <Course> courses) {
        this.name = name;
        this.courses = courses == null ? new ArrayList<>(0): courses;
    }

    @Override
    public String getName() {
        return name ;
        
    }

    @Override
    public List<Course> getAllCourses() {
        return courses;
    }

    @Override
    public String toString() {
        return name;
    }
}

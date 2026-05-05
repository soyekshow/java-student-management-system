import java.io.*;
import java.util.*;

class Student {
    String name;
    double grade;

    Student(String name, double grade) {
        this.name = name;
        this.grade = grade;
    }

    @Override
    public String toString() {
        return name + "|" + grade;
    }
}

public class Main {
    static ArrayList<Student> students = new ArrayList<>();
    static Scanner scanner = new Scanner(System.in);
    static final String FILE_NAME = "students.txt";

    public static void loadStudents() {
        try {
            File file = new File(FILE_NAME);
            if (!file.exists()) return;

            Scanner fileScanner = new Scanner(file);
            while (fileScanner.hasNextLine()) {
                String line = fileScanner.nextLine();
                String[] parts = line.split("\\|");
                students.add(new Student(parts[0], Double.parseDouble(parts[1])));
            }
            fileScanner.close();
        } catch (Exception e) {
            System.out.println("Dosya okunamadı.");
        }
    }

    public static void saveStudents() {
        try {
            PrintWriter writer = new PrintWriter(FILE_NAME);
            for (Student s : students) {
                writer.println(s.toString());
            }
            writer.close();
        } catch (Exception e) {
            System.out.println("Dosya yazılamadı.");
        }
    }

    public static void addStudent() {
        System.out.print("Öğrenci adı: ");
        String name = scanner.nextLine();

        System.out.print("Not: ");
        double grade = scanner.nextDouble();
        scanner.nextLine();

        students.add(new Student(name, grade));
        saveStudents();
        System.out.println("Eklendi.");
    }

    public static void showStudents() {
        if (students.isEmpty()) {
            System.out.println("Liste boş.");
            return;
        }

        for (int i = 0; i < students.size(); i++) {
            Student s = students.get(i);
            System.out.println((i+1) + ". " + s.name + " - " + s.grade);
        }
    }

    public static void deleteStudent() {
        showStudents();
        try {
            System.out.print("Silinecek numara: ");
            int index = scanner.nextInt();
            scanner.nextLine();

            students.remove(index - 1);
            saveStudents();
            System.out.println("Silindi.");
        } catch (Exception e) {
            System.out.println("Hatalı seçim.");
        }
    }

    public static void calculateAverage() {
        if (students.isEmpty()) {
            System.out.println("Liste boş.");
            return;
        }

        double sum = 0;
        for (Student s : students) sum += s.grade;

        System.out.println("Ortalama: " + (sum / students.size()));
    }

    public static void highestGrade() {
        if (students.isEmpty()) {
            System.out.println("Liste boş.");
            return;
        }

        Student best = students.get(0);
        for (Student s : students) {
            if (s.grade > best.grade) best = s;
        }

        System.out.println("En yüksek: " + best.name + " - " + best.grade);
    }

    public static void menu() {
        System.out.println("\n1- Öğrenci ekle");
        System.out.println("2- Listele");
        System.out.println("3- Ortalama");
        System.out.println("4- En yüksek not");
        System.out.println("5- Öğrenci sil");
        System.out.println("6- Çıkış");
    }

    public static void main(String[] args) {
        loadStudents();

        while (true) {
            menu();
            int choice = scanner.nextInt();
            scanner.nextLine();

            switch (choice) {
                case 1: addStudent(); break;
                case 2: showStudents(); break;
                case 3: calculateAverage(); break;
                case 4: highestGrade(); break;
                case 5: deleteStudent(); break;
                case 6: return;
                default: System.out.println("Hatalı seçim.");
            }
        }
    }
}

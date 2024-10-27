
import java.time.LocalDate;


public class salary extends Worker {

    private String status;
    private LocalDate date;

    public salary() {
    }

    public salary(String status, LocalDate date) {
        this.status = status;
        this.date = date;
    }

    public salary(String status, LocalDate date, String id, String name, int age, double salary, String Location) {
        super(id, name, age, salary, Location);
        this.status = status;
        this.date = date;
    }

    public String getStatus() {
        return status;
    }

    public void setStatus(String status) {
        this.status = status;
    }

    public LocalDate getDate() {
        return date;
    }

    public void setDate(LocalDate date) {
        this.date = date;
    }

    @Override
    public String toString() {
        return super.toString() + String.format("%-10s %-10s", status, date);
    }

}


public class Worker {

    private String id;
    private String name;
    private int age;
    private double salary;
    private String Location;

    public Worker() {
    }
    

    public Worker(String id, String name, int age, double salary, String Location) {
        this.id = id;
        this.name = name;
        this.age = age;
        this.salary = salary;
        this.Location = Location;
    }

    public String getId() {
        return id;
    }

    public void setId(String id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    public double getSalary() {
        return salary;
    }

    public void setSalary(double salary) {
        this.salary = salary;
    }

    public String getLocation() {
        return Location;
    }

    public void setLocation(String Location) {
        this.Location = Location;
    }

    @Override
    public String toString() {
         return String.format("%-10s %-15s %-10d %-15.2f", id, name, age, salary);
    }
    
    

}

import java.util.Scanner;

public class Menu {

    private Scanner sc = new Scanner(System.in);
    private Manageworker mw = new Manageworker();

    public void menu() {
        while (true) {
            System.out.println("======== Worker======");
            System.out.println("1. Add Worker");
            System.out.println("2. Up salary");
            System.out.println("3. Down salary");
            System.out.println("4. Display information salary");
            System.out.println("5. Exit");

            int choice = -1;
            while (true) {
                try {
                    System.out.print("");
                    choice = Integer.parseInt(sc.nextLine()); // Đọc toàn bộ dòng và chuyển đổi thành số nguyên
                    switch (choice) {
                        case 1:
                            mw.addWorker();
                            break;
                        case 2:
                            mw.manageSalary(1);
                            break;
                        case 3:
                            mw.manageSalary(2);
                            break;
                        case 4:
                            mw.display();
                            break;
                        case 5:
                            return;
                        default:
                            System.out.println("please input 1-5");
                    }
                    break; // Thoát khỏi vòng lặp nếu nhập hợp lệ
                } catch (NumberFormatException e) {
                    System.out.println("please input 1-5");
                }
            }

        }
    }
}

import java.time.LocalDate;
import java.util.ArrayList;
import java.util.List;
import java.util.Scanner;

public class Manageworker {

    private List<Worker> workerList;
    private List<salary> salaryList;
    private Scanner sc = new Scanner(System.in);

    public Manageworker() {
        workerList = new ArrayList<>();
        salaryList = new ArrayList<>();
    }

    public void addWorker() {
        System.out.println("-------Add Worker-------");

        while (true) {
            // Kiểm tra mã nhân viên    
            System.out.print("Enter code: ");
            String code = sc.nextLine().trim();
            boolean exists = false;
            if (code == null || code.isEmpty()) {
                System.out.println("Code cannot be null or empty.");
                continue;  // Yêu cầu nhập lại mã nhân viên
            }

            for (Worker worker : workerList) {
                if (worker.getId().equals(code)) {
                    System.out.println("Worker " + code + " already exists.");
                    exists = true;
                    break;
                }
            }

            if (exists) {
                continue;
            }

            // Kiểm tra tên nhân viên
            String name;
            while (true) {
                System.out.print("Enter name: ");
                name = sc.nextLine().trim();
                if (!name.matches("[a-zA-Z\\s]+") || name.isEmpty()) {
                    System.out.println(" Please enter a valid name !");
                } else {
                    break;
                }

            }

            // Kiểm tra tuổi nhân viên
            int age = 0;
            while (true) {
                try {
                    System.out.print("Enter age: ");
                    age = Integer.parseInt(sc.nextLine().trim());
                    if (age < 18 || age > 50) {
                        System.out.println("Age must be between 18 and 50.");
                        continue;
                    }
                    break;
                } catch (NumberFormatException e) {
                    System.out.println("Invalid age format. Please enter a valid integer.");
                }
            }

            // Kiểm tra lương nhân viên
            double salary = 0;
            while (true) {
                try {
                    System.out.print("Enter salary: ");
                    salary = Double.parseDouble(sc.nextLine().trim());
                    if (salary <= 0) {
                        System.out.println("Salary must be greater than 0.");
                        continue;
                    }
                    break;
                } catch (Exception e) {
                    System.out.println("Invalid salary format. Please enter a valid number.");
                }
            }

            // Kiểm tra địa điểm làm việc
            String location;
            while (true) {
                System.out.print("Enter work location: ");
                location = sc.nextLine().trim();
                if (location.isEmpty()) {
                    System.out.println("Work location cannot be empty. Please enter a valid location.");
                } else {
                    break; // Thoát khỏi vòng lặp nếu địa điểm hợp lệ
                }
            }

            // Thêm nhân viên vào danh sách
            Worker newWorker = new Worker(code, name, age, salary, location);
            workerList.add(newWorker);
            System.out.println("Worker added successfully.");
            break;  // Thoát khỏi vòng lặp sau khi thêm thành công
        }
    }

    public void manageSalary(int mode) {
        if (mode == 1) {
            while (true) {
                System.out.print("Enter code: ");
                String code = sc.nextLine().trim();

                // Kiểm tra xem code có để trống không
                if (code.isEmpty()) {
                    System.out.println("Code cannot be empty. Please enter a valid code.");
                    continue; // Yêu cầu người dùng nhập lại
                }

                // Tìm kiếm nhân viên theo mã code
                Worker worker = null;
                for (Worker foundWorker : workerList) {
                    if (foundWorker.getId().equals(code)) {
                        worker = foundWorker;
                        break;
                    }
                }

                // Nếu không tìm thấy nhân viên với mã code, yêu cầu nhập lại
                if (worker == null) {
                    System.out.println("No worker found with code: " + code);
                    continue; // Yêu cầu người dùng nhập lại
                }

                double amount = 0;
                while (true) {
                    try {
                        System.out.print("Enter Salary: ");
                        amount = Double.parseDouble(sc.nextLine());
                        if (amount <= 0) {
                            System.out.println("Salary must be greater than 0.");
                            continue;
                        }
                        break;
                    } catch (NumberFormatException e) {
                        System.out.println("Salary must be a number.");
                    }
                }
                double newsalary = worker.getSalary() + amount;
                worker.setSalary(newsalary);
                String status = "UP";
                salary s = new salary(status, LocalDate.now(), worker.getId(), worker.getName(), worker.getAge(), newsalary, worker.getLocation());
                // Kiểm tra xem nhân viên đã có bản ghi lương trong salaryList chưa
                boolean salaryExists = false;
                for (int i = 0; i < salaryList.size(); i++) {
                    if (salaryList.get(i).getId().equals(worker.getId())) {
                        // Nếu đã tồn tại, cập nhật bản ghi lương mới
                        salaryList.set(i, s);
                        salaryExists = true;
                        break;
                    }
                }
// Nếu không có bản ghi nào, thêm bản ghi mới vào danh sách
                if (!salaryExists) {
                    salaryList.add(s);
                }

                System.out.println("Salary update successfully.");
                break;  // Thoát khỏi vòng lặp sau khi cập nhật thành công
            }
        } else if (mode == 2) {
            while (true) {
                System.out.print("Enter code: ");
                String code = sc.nextLine().trim();

                // Kiểm tra xem code có để trống không
                if (code.isEmpty()) {
                    System.out.println("Code cannot be empty. Please enter a valid code.");
                    continue; // Yêu cầu người dùng nhập lại
                }

                // Tìm kiếm nhân viên theo mã code
                Worker worker = null;
                for (Worker foundWorker : workerList) {
                    if (foundWorker.getId().equals(code)) {
                        worker = foundWorker;
                        break;
                    }
                }

                // Nếu không tìm thấy nhân viên với mã code, yêu cầu nhập lại
                if (worker == null) {
                    System.out.println("No worker found with code: " + code);
                    continue; // Yêu cầu người dùng nhập lại
                }

                double amount = 0;
                while (true) {
                    try {
                        System.out.print("Enter Salary : ");
                        String input = sc.nextLine().trim();

                        // Kiểm tra nếu input rỗng
                        if (input.isEmpty()) {
                            System.out.println("Error: Input cannot be empty. Please enter a valid amount.");
                            continue; // Quay lại vòng lặp để nhập lại
                        }

                        // Kiểm tra xem input có phải là số hợp lệ không
                        amount = Double.parseDouble(input);

                        // Kiểm tra nếu amount nhỏ hơn 0
                        if (amount < 0) {
                            System.out.println("Error: Amount must be greater than or equal to 0.");
                            continue; // Quay lại vòng lặp để nhập lại
                        }

                        // Tính toán lương mới
                        double newsalary = worker.getSalary() - amount;

                        // Kiểm tra nếu lương mới nhỏ hơn 0
                        if (newsalary < 0) {
                            System.out.println("Error: New salary cannot be less than 0. Please enter a valid deduction amount.");
                            continue;  // Quay lại vòng lặp để nhập lại amount
                        }

                        // Nếu lương hợp lệ thì tiếp tục cập nhật lương
                        worker.setSalary(newsalary);
                        String status = "Down";
                        salary s = new salary(status, LocalDate.now(), worker.getId(), worker.getName(), worker.getAge(), newsalary, worker.getLocation());
                        // Kiểm tra xem nhân viên đã có bản ghi lương trong salaryList chưa
                        boolean salaryExists = false;
                        for (int i = 0; i < salaryList.size(); i++) {
                            if (salaryList.get(i).getId().equals(worker.getId())) {
                                // Nếu đã tồn tại, cập nhật bản ghi lương mới
                                salaryList.set(i, s);
                                salaryExists = true;
                                break;
                            }
                        }
// Nếu không có bản ghi nào, thêm bản ghi mới vào danh sách
                        if (!salaryExists) {
                            salaryList.add(s);
                        }

                        System.out.println("Salary update successfully.");
                        return;  // Thoát khỏi vòng lặp sau khi cập nhật thành công

                    } catch (NumberFormatException e) {
                        System.out.println("Error: Amount must be a valid number.");
                    }
                }
            }
        }
    }

    public void display() {
        String header = String.format("%-10s %-15s %-10s %-15s %-8s %-12s", "Code", "Name", "Age", "Salary", "Status", "Date");
        System.out.println(header);

        // Duyệt qua danh sách nhân viên
        for (Worker worker : workerList) {
            salary foundSalary = null;

            // Tìm kiếm thông tin cập nhật lương của nhân viên trong danh sách salaryList
            for (salary s : salaryList) {
                if (s.getId().equals(worker.getId())) {
                    foundSalary = s;
                    break;
                }
            }

            // Nếu không tìm thấy thông tin cập nhật lương, đặt status và date là null
            if (foundSalary == null) {
                System.out.println(String.format("%-10s %-15s %-10d %-15.2f %-8s %-12s",
                        worker.getId(), worker.getName(), worker.getAge(), worker.getSalary(), "null", "null"));
            } else {
                // Nếu đã cập nhật lương, hiển thị đầy đủ thông tin
                System.out.println(foundSalary.toString());
            }
        }
    }
}
/*
 * Click nbfs://nbhost/SystemFileSystem/Templates/Licenses/license-default.txt to change this license
 * Click nbfs://nbhost/SystemFileSystem/Templates/Classes/Class.java to edit this template
 */

/**
 *
 * @author ntanh
 */
public class Main {

    public static void main(String[] args) {
        Menu m = new Menu();
        m.menu();
                
    }
}

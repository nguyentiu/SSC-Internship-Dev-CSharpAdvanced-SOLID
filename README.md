# SSC-Internship-Dev-CSharpAdvanced-SOLID
# Hướng dẫn về SOLID trong C#
## 1. Giới thiệu về Nguyên tắc SOLID
SOLID là từ viết tắt đại diện cho năm nguyên tắc thiết kế quan trọng trong lập trình hướng đối tượng, nhằm mục đích tạo ra phần mềm dễ hiểu, linh hoạt và dễ bảo trì hơn. Những nguyên tắc này giúp lập trình viên tránh những cạm bẫy phổ biến trong thiết kế phần mềm, như sự kết nối chặt chẽ và độ cứng nhắc của mã, giúp mã nguồn dễ quản lý và mở rộng hơn theo thời gian.

Năm nguyên tắc SOLID bao gồm:

1. Single Responsibility Principle (SRP) - Nguyên tắc Trách nhiệm duy nhất
2. Open/Closed Principle (OCP) - Nguyên tắc Mở/Đóng
3. Liskov Substitution Principle (LSP) - Nguyên tắc Thay thế Liskov
4. Interface Segregation Principle (ISP) - Nguyên tắc Phân tách Interface
5. Dependency Inversion Principle (DIP) - Nguyên tắc Đảo ngược Sự phụ thuộc
## 2. Giải thích chi tiết từng nguyên tắc SOLID
### 2.1. Single Responsibility Principle (SRP)
Single Responsibility Principle (SRP) hay còn gọi là nguyên tắc Trách nhiệm duy nhất, quy định rằng mỗi class chỉ nên có một lý do duy nhất để thay đổi, nghĩa là mỗi class chỉ nên đảm nhiệm một trách nhiệm duy nhất. Nguyên tắc này giúp mã nguồn trở nên rõ ràng và dễ bảo trì hơn vì một class chỉ tập trung vào một nhiệm vụ cụ thể.

Ví dụ:

```csharp
public class StudentManager
{
    public void AddStudent(Student student) { /* Thêm sinh viên */ }
    public void GenerateStudentReport(Student student) { /* Tạo báo cáo cho sinh viên */ }
    public void SendStudentEmail(Student student) { /* Gửi email cho sinh viên */ }
}
```
Vi phạm SRP: `Class StudentManager` có quá nhiều trách nhiệm
```csharp
public class StudentManager
{
    public void AddStudent(Student student) { /* Thêm sinh viên */ }
}

public class StudentReportGenerator
{
    public void GenerateStudentReport(Student student) { /* Tạo báo cáo cho sinh viên */ }
}

public class StudentEmailService
{
    public void SendStudentEmail(Student student) { /* Gửi email cho sinh viên */ }
}
```
Tuân thủ SRP: Tách các trách nhiệm thành các class riêng biệt
## 2.2. Open/Closed Principle (OCP)
Open/Closed Principle (OCP), hay còn gọi là nguyên tắc Mở/Đóng, quy định rằng các module của phần mềm nên được mở để mở rộng nhưng đóng để thay đổi. Điều này có nghĩa là bạn có thể thêm chức năng mới mà không cần thay đổi mã nguồn hiện tại, giúp mã dễ bảo trì và mở rộng hơn.

Ví dụ:

```csharp
public class Calculation
{
    public double Calculate(string operation, double x, double y)
    {
        switch (operation)
        {
            case "add": return x + y;
            case "subtract": return x - y;
            // ...
            default: throw new InvalidOperationException("Operation not supported");
        }
    }
}
```
Vi phạm OCP: `Class Calculation` bị thay đổi mỗi khi cần thêm phép toán mới
```csharp
public abstract class Operation
{
    public abstract double Execute(double x, double y);
}

public class AddOperation : Operation
{
    public override double Execute(double x, double y) => x + y;
}

public class SubtractOperation : Operation
{
    public override double Execute(double x, double y) => x - y;
}

public class Calculation
{
    public double Calculate(Operation operation, double x, double y) => operation.Execute(x, y);
}
```
Tuân thủ OCP: Mở rộng class bằng cách thêm các class kế thừa mà không thay đổi mã nguồn hiện tại
## 2.3. Liskov Substitution Principle (LSP)
Liskov Substitution Principle (LSP), hay còn gọi là nguyên tắc Thay thế Liskov, quy định rằng các đối tượng của lớp con phải có thể thay thế cho lớp cha của chúng mà không làm thay đổi tính đúng đắn của chương trình. Điều này đảm bảo rằng các lớp con có thể được sử dụng thay thế cho các lớp cha mà không gây ra lỗi hoặc hành vi không mong muốn.

Ví dụ:

```csharp
public class Rectangle
{
    public virtual int Width { get; set; }
    public virtual int Height { get; set; }
    public int Area() => Width * Height;
}

public class Square : Rectangle
{
    public override int Width
    {
        set { base.Width = base.Height = value; }
    }

    public override int Height
    {
        set { base.Width = base.Height = value; }
    }
}
```
Vi phạm LSP: Class `Square` không thể thay thế hoàn toàn cho class `Rectangle`
```csharp
public abstract class Shape
{
    public abstract int Area();
}

public class Rectangle : Shape
{
    public int Width { get; set; }
    public int Height { get; set; }
    public override int Area() => Width * Height;
}

public class Square : Shape
{
    public int Side { get; set; }
    public override int Area() => Side * Side;
}
```
Tuân thủ LSP: Tách `Square` và `Rectangle` thành các class riêng biệt
### 2.4. Interface Segregation Principle (ISP)
Interface Segregation Principle (ISP), hay còn gọi là nguyên tắc Phân tách Interface, quy định rằng không nên buộc các lớp triển khai các interface mà chúng không sử dụng. Thay vào đó, hãy tách các interface lớn thành các interface nhỏ hơn, cụ thể hơn, để các lớp chỉ cần triển khai những gì cần thiết.

Ví dụ:

```csharp
public interface IMachine
{
    void Print(string content);
    void Scan(string content);
}

public class DocumentPrinter : IMachine
{
    public void Print(string content) { /* In tài liệu */ }
    public void Scan(string content) { throw new NotImplementedException(); }
}
```
Vi phạm ISP: `Class DocumentPrinter` bị buộc phải triển khai `Print` và `Scan` dù không cần
```csharp
public interface IPrinter
{
    void Print(string content);
}

public interface IScanner
{
    void Scan(string content);
}

public class DocumentPrinter : IPrinter
{
    public void Print(string content) { /* In tài liệu */ }
}
```
Tuân thủ ISP: Tách interface thành các interface nhỏ hơn
## 2.5. Dependency Inversion Principle (DIP)
Dependency Inversion Principle (DIP), hay còn gọi là nguyên tắc Đảo ngược Sự phụ thuộc, quy định rằng các module cấp cao không nên phụ thuộc vào các module cấp thấp. Cả hai nên phụ thuộc vào các abstraction (interface hoặc abstract class). Điều này giúp giảm sự kết nối chặt chẽ và tăng tính linh hoạt cho hệ thống.

Ví dụ:

```csharp
public class EmailService
{
    public void SendEmail(string message) { /* Gửi email */ }
}

public class PasswordChecker
{
    private EmailService _emailService = new EmailService();

    public void CheckPassword(string password)
    {
        if (password == "1234")
        {
            _emailService.SendEmail("Password is weak");
        }
    }
}
```
Vi phạm DIP: `Class PasswordChecker` phụ thuộc trực tiếp vào `class EmailService`
```csharp
public interface IEmailService
{
    void SendEmail(string message);
}

public class EmailService : IEmailService
{
    public void SendEmail(string message) { /* Gửi email */ }
}

public class PasswordChecker
{
    private IEmailService _emailService;

    public PasswordChecker(IEmailService emailService)
    {
        _emailService = emailService;
    }

    public void CheckPassword(string password)
    {
        if (password == "1234")
        {
            _emailService.SendEmail("Password is weak");
        }
    }
}
```
Tuân thủ DIP: Sử dụng interface để giảm sự phụ thuộc
## 6. Tổng kết
Nguyên tắc SOLID là những hướng dẫn mạnh mẽ giúp bạn viết mã sạch, dễ bảo trì và mở rộng. Bằng cách tuân thủ các nguyên tắc này, bạn có thể thiết kế phần mềm có cấu trúc tốt, ít lỗi và dễ dàng thích ứng với sự thay đổi trong tương lai.

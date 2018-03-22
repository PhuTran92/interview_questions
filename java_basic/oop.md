## 1. class là gì? là một template định nghĩa và mô tả thuộc tính và hành vi của object 

> Encapsulation (tính bao đóng): có nghĩa là các thuộc tính nội bộ của object sẽ được hidden với các đối tượng khác bên ngoài. Nếu muốn truy xuất các thuộc tính này thì object phải cung cấp cái method set,get cho các đối tượng bên ngoài truy xuất.

Abstraction (tính trừu tượng): là tính chất sử dụng để tổng quát hoá các đặc tính của một object trong thực tiến, định danh và phân biệt nó với các object khác

Inheritance: tính chất thể hiện mối quan hệ is-a của một object. Hỗ trợ reuse code.

Polymorphism: là tính chất hiểu đơn giản là one name many form. Một method nhưng có nhiều định nghĩa hiện thực khác nhau 

Có 2 loại đa hình là 

overloading:  static, compile time Polymorphism

cùng method name nhưng khác số lượng đối số truyền vào, kiểu dữ liệu của đối số, thứ tự đối số

khác kiểu dữ liệu trả về với cùng method name thì ko dc 

It's not possible to have a method with same parameters and different return type. Compiler throws error in the below case(Duplicate method).
	Method 1 :
	public int calc(int a, int b, int c)
	Method 2 :
	public String calc(int e, int f, int g)


và overriding: dynamic, runtime Polymorphism

class con kế thừa method từ lớp cha và override lại method của lớp cha

S.O.L.I.D principles

Single responsibility principle
A class should have one and only one reason to change, meaning that a class should only have one job.

```java
	class Employee {
	  public Pay calculatePay() {...}
	  public void save() {...}
	  public String describeEmployee() {...}
	}
```
	Open-Closed Principle
	open for extension but close for modification
	Thể hiện thông qua kế thừa (interface hoặc class)


	Liskov Substitution Principle
	Nếu class A là con của class B thì object type B có thể dc thay thế bởi object type A


	Interface Segregation Principle
	A client should never be forced to implement an interface that it doesn’t use or clients shouldn’t be forced to depend on methods they do not use.


	The Dependency Inversion Principle
	Depend on abstractions, not on concretions.

	Phòng tránh dc trường hợp thay đổi Reader and Writer ko phải modify lại code 

	public interface Reader { char getchar(); }
	public interface Writer { void putchar(char c)}

	class CharCopier {

	  void copy(Reader reader, Writer writer) {
	    int c;
	    while ((c = reader.getchar()) != EOF) {
	      writer.putchar();
	    }
	  }
	}

	public Keyboard implements Reader {...}
	public Printer implements Writer {...}


---------------------

abstract class
	có cả hàm trừu tượng và hàm dc hiện thực 
	ko thể new dc, phải dc extend bởi lớp con mới xài dc

interface 
	như một là template của một class, chỉ chứa các method trừu tượng mài thôi, chứa những hàm common của các lớp con. Phải extend mới xài dc 


serialization
	Serialization là quá trình chuyển đổi object thành một chuỗi bytes để lưu lại trên bộ nhớ và recreate lại sau.	

== vs .equals()
	1) Main difference between .equals() method and == operator is that one is method and other is operator.

	2) We can use == operators for reference comparison (address comparison) and .equals() method for content comparison. In simple words, == checks if both objects point to the same memory location whereas .equals() evaluates to the comparison of values in the objects.

	In general the equality operator in Java performs a so called shallow comparison. In other words it compares the values that variables contains. Now the variables of primitive data types contains the value itself while the reference types contains reference to heap area which stores the actual content. That means that in your code snippet int b will hold value 1 and Integer a will hold the memory address of the actual Integer object on the heap. 

	3) If a class does not override the equals method, then by default it uses equals(Object o) method of the closest parent class that has overridden this method. See this for detail

Why would you not call abstract method in constructor?
	null pointer


Final is a keyword in the java language. It is used to apply restrictions on class, method and variable. Final class can't be inherited, final method can't be overridden and final variable value can't be changed.

Finally is a code block and is used to place important code, it will be executed whether exception is handled or not. 

Finalize is a method used to perform clean up processing just before object is garbage collected.



Can a static method be overridden in Java

While child class can override a static method with another static method with the same signature (return type can be downcasted), it is not truly overridden - it becomes "hidden"

	static class Class1 {
	    public static int Method1(){
	          return 0;
	    }
	}
	static class Class2 extends Class1 {
	    public static int Method1(){
	          return 1;
	    }

	}
	public static class Main {
	    public static void main(String[] args){
	          //Must explicitly chose Method1 from Class1 or Class2
	          Class1.Method1();
	          Class2.Method1();
	    }
	}

JAVA is past by value 


Volatile keyword of making class thread safe. Thread safe means that a method or class instance can be used by multiple threads at the same time without any problem.
Lưu biến dc khai báo với từ khoá Volatile sẽ chỉ dc lưu trên main memory, tất cả các thread sử dụng biến này ko cần make a copy version of it. Nhờ đó khi giá trị biến thay đổi thì tất cả các thread sử dụng biến này cũng dc aware


Từ khoán c cũng hỗ trợ thread safe. như cho method hoặc block of code.
[https://www.geeksforgeeks.org/volatile-keyword-in-java/](https://www.geeksforgeeks.org/volatile-keyword-in-java/)

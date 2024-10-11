# SOLID-training
Single Resposibility Principle
Open-Closed Principle
Liskov Substitution Principle
Interface Segregation Principle
Dependecy Injection Principle

- Software components should be open for extension, but closed for modification

* Open for Extension: A software component should be extendable to add new feature or to add new behavior to it
* Closed for modification: New features getting added to the software component, should NOT have to modify existing code.

## Single Responsibility Princile (SRP)
Every software component should have one and only one resposibility

_Cohesion_: The degree to which the various parts of a software component are related

Public class Square {

    int side = 5;

    public int calculateArea(){
        return side * side;
    }

    public int calculatePerimeter() {
        return side * 4;
    }

    public void draw(){
        if (jogResoluitionMonitor){
            // Render a high resolution image of a square        
        } else {
            // Render a normal image of a square
        }
    }

    public void rotate(int degree){
        // Rotate the image of the square clockwise to
        // the required degree and re-render
    }

}

The methods for calculation and rendering do not have a high level of cohesion between them. Separating them into different classes improves the cohesion as a whole

Public class Square {

    int side = 5;

    public int calculateArea(){
        return side * side;
    }

    public int calculatePerimeter() {
        return side * 4;
    }

}

public class SquareUI {

    public void draw(){
        if (jogResoluitionMonitor){
            // Render a high resolution image of a square        
        } else {
            // Render a normal image of a square
        }
    }

    public void rotate(int degree){
        // Rotate the image of the square clockwise to
        // the required degree and re-render
    }

}

Higher Cohesion helps attain better adherence to the Single Responsibility Principle. Thus, assigning a single responsibility to each module. 

_Coupling_: The level of inter dependency between various software components

Puiblic class Student {
    
    Private String studentId;
    private Date studentDOB;
    private String address;

    public void save() {
        // Serialize object into a string representation
        String objectStr = MyUtils.seralizeIntoAString(this);
        Connection connection = null;
        Statement stmt = null;
        try{
            Class.forName("com.mysql.jdbc.Driver");
            connection = DriverManager.getConnection("jdbc:mysql://Localhost:3306/MyDb", "root", "password")
            stmt = connection.createStatement();
            stmt.execute("INSERT INTO STUDENT VALUES (" + objectStr + ")");
        } catch (Exception e) {
                e.printStackTrace();
        }
    }

    public String getStudentId() {
        return studentId;
    }

    public void setStudentId(String studentId) {
        this.strudentId = studentId;
    }
}

In the event that the way the database is handled is changes, i.e from mysql to mongodb, the save method will have to go through many changes. It is tight coupled to mysql.
Puiblic class Student {
    
    Private String studentId;
    private Date studentDOB;
    private String address;

    public void save() {
        new StudentRepository().save(this);
    }

    public String getStudentId() {
        return studentId;
    }

    public void setStudentId(String studentId) {
        this.strudentId = studentId;
    }
}

Public class StudentRepository {
    public void save() {
        // Serialize object into a string representation
        String objectStr = MyUtils.seralizeIntoAString(this);
        Connection connection = null;
        Statement stmt = null;
        try{
            Class.forName("com.mysql.jdbc.Driver");
            connection = DriverManager.getConnection("jdbc:mysql://Localhost:3306/MyDb", "root", "password")
            stmt = connection.createStatement();
            stmt.execute("INSERT INTO STUDENT VALUES (" + objectStr + ")");
        } catch (Exception e) {
                e.printStackTrace();
        }
    }

}

By defining a repository which handles database connections any change in the way the database is handled is nos responsibility of said repository.

__Loose Coupling helps attain better adherence to the Single Responsibility Principle.__

### Reasons for Change
Every software component should have one and only one _reason_ to change. Software is never dormant, it always keeps changing.

Puiblic class Student {
    
    Private String studentId;
    private Date studentDOB;
    private String address;

    public void save() {
        // Serialize object into a string representation
        String objectStr = MyUtils.seralizeIntoAString(this);
        Connection connection = null;
        Statement stmt = null;
        try{
            Class.forName("com.mysql.jdbc.Driver");
            connection = DriverManager.getConnection("jdbc:mysql://Localhost:3306/MyDb", "root", "password")
            stmt = connection.createStatement();
            stmt.execute("INSERT INTO STUDENT VALUES (" + objectStr + ")");
        } catch (Exception e) {
                e.printStackTrace();
        }
    }

    public String getStudentId() {
        return studentId;
    }

    public void setStudentId(String studentId) {
        this.strudentId = studentId;
    }
}

Reasons to change
1.  A change in the student id format
2.  A change in the student name format.
3.  A change in the database backend, as advised by the technical team.

If a software component has multiple reasons to change, then the frequency of changes to it will increase, thus increasing the posibility to introduce new bugs.

Puiblic class Student {
    
    Private String studentId;
    private Date studentDOB;
    private String address;

    public void save() {
        new StudentRepository().save(this);
    }

    public String getStudentId() {
        return studentId;
    }

    public void setStudentId(String studentId) {
        this.strudentId = studentId;
    }
}

Reasons to change
1.  Changes to student profile

Public class StudentRepository {
    public void save() {
        // Serialize object into a string representation
        String objectStr = MyUtils.seralizeIntoAString(this);
        Connection connection = null;
        Statement stmt = null;
        try{
            Class.forName("com.mysql.jdbc.Driver");
            connection = DriverManager.getConnection("jdbc:mysql://Localhost:3306/MyDb", "root", "password")
            stmt = connection.createStatement();
            stmt.execute("INSERT INTO STUDENT VALUES (" + objectStr + ")");
        } catch (Exception e) {
                e.printStackTrace();
        }
    }

}

Reasons to change
1.  A change in the database backend, as advised by the technical team.


### Live coding Session
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.Statement;

public class Employee {
    private String employeeId;
    private String employeeName;
    private String employeAddress;
    pruvate String contactNumber;
    private String employeeType;

    public void save() {
        // Serialize object into a string representation
        String objectStr = MyUtils.seralizeIntoAString(this);
        Connection connection = null;
        Statement stmt = null;
        try{
            Class.forName("com.mysql.jdbc.Driver");
            connection = DriverManager.getConnection("jdbc:mysql://Localhost:3306/MyDb", "root", "password")
            stmt = connection.createStatement();
            stmt.execute("INSERT INTO STUDENT VALUES (" + objectStr + ")");
        } catch (Exception e) {
                e.printStackTrace();
        }
    }

    public void calculateTax() {
        if (this.getEmployeeType().equals("fulltime")) {
            // Tax calc for full time employee
        }
        if (this.getEmployeeType().equals("contract")) {
            // Tax calc for contractors
        }
    }

    public String getEmployeeId() {
        return employeeId;
    }

    public string setEmployeeId(String employeeId){
        this.employeeId = employeeId;
    }

    public String getEmployeeName() {
        return employeeName;
    }

    public class setEmployeeName(String employeeName) {
        this.employeeName = employeeName;
    }

    public String getEmployeeAddress() {
        return employeeAddress
    }

    public void setEmployeeAddress(String employeeAddress){
        this.employeeAddress = employeeAddress;
    }

    public String getcontactNumber() {
        return contactNumber;
    }

    public void setContactNumber(String contactNumber){
        this.contactNumber = contactNumber
    }

    public String getEmployeeType() {
        return employeeType;
    }

    public void setEmployeeType(String employeeType){
        this.employeetype = employeeType;
    }

}

Reasons to change
1.  Changes in Employee Attributes
2.  Changes in Database
3.  Changes in Tax Calculation


public class Employee {
    private String employeeId;
    private String employeeName;
    private String employeAddress;
    pruvate String contactNumber;
    private String employeeType;

    public void save() {
        new EmployeeRepository().Save(this);
    }
    
    public void calculateTax() {
        new TaxCaluclator().calculateTax(this);
    }

    public String getEmployeeId() {
        return employeeId;
    }

    public string setEmployeeId(String employeeId){
        this.employeeId = employeeId;
    }

    public String getEmployeeName() {
        return employeeName;
    }

    public class setEmployeeName(String employeeName) {
        this.employeeName = employeeName;
    }

    public String getEmployeeAddress() {
        return employeeAddress
    }

    public void setEmployeeAddress(String employeeAddress){
        this.employeeAddress = employeeAddress;
    }

    public String getcontactNumber() {
        return contactNumber;
    }

    public void setContactNumber(String contactNumber){
        this.contactNumber = contactNumber
    }

    public String getEmployeeType() {
        return employeeType;
    }

    public void setEmployeeType(String employeeType){
        this.employeetype = employeeType;
    }

}

_Reasons to change_
1.  Changes in Employee Attributes

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.Statement;
public class EmployeeRepository { 
    public void save(Employee employee) {
        // Serialize object into a string representation
        String objectStr = MyUtils.seralizeIntoAString(employee);
        Connection connection = null;
        Statement stmt = null;
        try{
            Class.forName("com.mysql.jdbc.Driver");
            connection = DriverManager.getConnection("jdbc:mysql://Localhost:3306/MyDb", "root", "password")
            stmt = connection.createStatement();
            stmt.execute("INSERT INTO STUDENT VALUES (" + objectStr + ")");
        } catch (Exception e) {
                e.printStackTrace();
        }
    }
}

_Reasons to change_
1.  Changes in Database

public class TaxCalculator {
        public void calculateTax(Employee employee) {
        if (employee.getEmployeeType().equals("fulltime")) {
            // Tax calc for full time employee
        }
        if (employee.getEmployeeType().equals("contract")) {
            // Tax calc for contractors
        }
    }
}

_Reasons to change_
1.  Changes in Tax Calculation

## Open Closed Principle
_Open Closed Principle (OCP)_: Software components should be closed for modification, but open for extension
_Closed For Modification_: New features getting added to the software component, should NOT have to modify existing code.
_Open For Extension_: A software component should be extendable to add a new feature or to add a new behavior to it.

Public class InsurancePremiumDiscountCalculator {

    public int
    calculatePremiumDiscountPercent(HealthInsuranceCustomerProfile customer){
        if (customer.isLoyalCustomer()){
                return 20;
        }
        return 0;
    }

}

public class HealthInsuranceCustomerProfile {
    public boolean isLoyalCustomer() {
        return true; //or false
    }
}

Should a Vehicle profile discount be added a new class can is added

public class VehicleInsuranceCustomerProfile {

    public boolean isLoyalCusotmer(){
        return true; //or false
    }
   
}

Now the Calculator class has to be modified since the calculate method currently takes in a HealthInsuranceCustomerProfile object. the only way out is to add a new overloaded method which takes in a VehicleInsuranceCustomerProfile object


Public class InsurancePremiumDiscountCalculator {

    public int
    calculatePremiumDiscountPercent(HealthInsuranceCustomerProfile customer){
        if (customer.isLoyalCustomer()){
                return 20;
        }
        return 0;
    }

    calculatePremiumDiscountPercent(VehicleInsuranceCustomerProfile customer){
        if (customer.isLoyalCustomer()){
                return 20;
        }
        return 0;
    }

}

What if we want to handle home insurance too? More code has to be added to this class, patching the code. This makes the code not closed for modification.

Public class InsurancePremiumDiscountCalculator {

    public int
    calculatePremiumDiscountPercent(CustomerProfile customer){
        if (customer.isLoyalCustomer()){
                return 20;
        }
        return 0;
    }

}

public interface CustomerProfile {
    public boolean isLoyalCustomer();
}

public class HealthInsuranceCustomerProfile 
Implements customerProfile {
    @Override
    public boolean isLoyalCustomer() {
        return true; //or false
    }
}

public class VehicleInsuranceCustomerProfile 
Implements customerProfile {

    @Override
    public boolean isLoyalCusotmer(){
        return true; //or false
    }
}

By adding the new class implementing the CustomerProfile interface the lass InsurancePremiumDiscountCalculator doesn't have to be change if whe ant to handle future extensions Thus, making it closed for modification.

### Key Takeaways

·   Ease of adding new features
·   Leads to minimal cost of developing and testing software
·   Open Closed Principle often requires decoupling, which, in turn, automatically follows the Single Responsibility Principle
    ·   SOLID prinicples are all intertwined and interdependent
    ·   SOLID principles are most effective when they are combined together

_Caution_
·   Do not follow the open Closed Principle bilndly
·   You will end up with a huge number of classes that can complicate your overall design
·   Make a subjective, rather than an objective decision.


### Live coding Session

// InsurancePremiumDiscountCaluclator.java
Public class InsurancePremiumDiscountCalculator {

    public int calculatePremiumDiscountPercent(HealthInsuranceCustomerProfile customer){
        if (customer.isLoyalCustomer()){
                return 20;
        }
        return 0;
    }

    public int calculatePremiumDiscountPercent(VehicleInsuranceCustomerProfile customer){
        if (customer.isLoyalCustomer()){
                return 20;
        }
        return 0;
    }

}

// HealthInsuranceCustomerProfile.java
import java.util.Random;

public class HealthInsuraceCustomerProfile {

    public boolean isLoyalCustomer() {
        return new Random().nextBoolean();
    }

}

// VehicleInsuranceCustomerProfile.java
import java.util.Random;

public class VehicleInsuyranceCustomerProfile {

    public boolean isLoyalCustomer() {
        return new Random().nextBoolean();
    }

}

Let's revamp the design so as to follow OCP.

// InsurancePremiumDiscountCaluclator.java
Public class InsurancePremiumDiscountCalculator {

    public int calculatePremiumDiscountPercent(CustomerProfile customer){
        if (customer.isLoyalCustomer()){
                return 20;
        }
        return 0;
    }

}

// HealthInsuranceCustomerProfile.java
import java.util.Random;

public class HealthInsuraceCustomerProfile implements CustomerProfile {

    public boolean isLoyalCustomer() {
        return new Random().nextBoolean();
    }

}

// VehicleInsuranceCustomerProfile.java
import java.util.Random;

public class VehicleInsuyranceCustomerProfile implements CustomerProfile {

    public boolean isLoyalCustomer() {
        return new Random().nextBoolean();
    }

}

//CustomerProfile.java

public interface CustomerProfile {
        public boolean isLoyalCustomer();
}

Let's add a new class to handle home insurance

//HomeInsuranceCustomerProfile
import java.util.Random;

public class HomeInsuyranceCustomerProfile implements CustomerProfile {

    public boolean isLoyalCustomer() {
        return new Random().nextBoolean();
    }
}

Notice that we did not have to change any other class to handle home insurance. We simply added a new class.


## Liskov Substitution Principle
_Liskov Substitution Principle (LSP)_: Objects should be replaceable with their subtypes without affecting the correctness of the program

In terms of inheritance, an Ostrich is a bird, however, Ostrich cannot fly. Unimplemented methds are almost always indicative of a design flaw.

public class Bird {
    public void fly() {
        //Fly
    }
}

public class Ostrich extends Bird {

    @Override
    public void fly(){
        // Unimplemented
        throw new RuntimeException();
    }
}

The statement Ostrich 'is-a' bird might still be correct. However, if we apply LSP here, this test fails. So LSP requires a test standard that is more strict than the 'is-a' test.
_Change the 'Is-A' way of thinking_. _If it looks like a duck and quacks like a duck but it needs batteries, you probably have the wrong abstaction!_

### Breaking the Hierarchy

public class Car {

    public double getCabinWidth(){
        // return cabin width
    }
}

public class RacingCar extends Car {

    @Override
    public double getCabinWidth() {
        //UNIMPLEMENTED!
    }

    public double getCockpitWidth() {
        // return cockpit width
    }

}

Racing car inherits from Car but racing cars do not have a cabin width. But they have a cockpit width.

Let's create some objects of Cars and RacingCars.

public class CarUtils {

    public static void main(String[] args) {
        Car first = new Car();
        Car second = new Car();
        car third = new RacingCar();

        List<Car> myCars = new ArrayList<>();
        myCars.add(first);
        myCars.add(second);
        myCars.add(third);

        for(Car car: myCars) {
            Sustem.out.printLn(car.getCabinWidth());
        }
    }

}

This code will not work correctly, because of an unimplemented method. To fix this the inheritance has to be broken.


public class Vehicle {

    public double getInteriorWidth() {
        //return interior width
    }

}

public class Car extends Vehicle {

    @Override
    public double getInteriorWidth() {
        return this.getCabinWidth();
    }

    public double getCabinWidth() {
        //return cabin width
    }

}

public class RacingCar extends Vehicle {

    @Override
    public double getInteriorWidth() {
        return this.getCockpitWidth()
    }

    public double getCockpitWidth() {
        //return cockpit width
    }
}

public class VehicleUtils {

    public static void main(String[] args) {
        Car first = new Car();
        Car second = new Car();
        car third = new RacingCar();

        List<Vehicle> myVehicles = new ArrayList<>();
        myVehicles.add(first);
        myVehicles.add(second);
        myVehicles.add(third);

        for(Vehicle vehicle: myVehicles) {
            Sustem.out.printLn(car.getInteriorWidth());
        }
    }

}

### Tell don't Ask
Let's apply LSP in a different way

public class Product {

    protected double discount;

    public double getDiscount() {
        return discount;
    };

}

public class InHouseProduct extends Product
{
    public void applyExtraDiscount() {
        discount = discount * 1.5;
    }
}

public class PricingUtils {

    public static void main(String[] args) {
        
        Product p1 = new Product();
        Product p2 = new Product();
        Product p3 = new InHouseProduct();

        List<Product> productList = new ArrayList<>();

        productList.add(p1);
        productList.add(p2);
        productList.add(p3);

        for (Product product : productList) {
            if (product instanceof InHouseProduct) {
                ((InHouseProduct) product).applyExtraDiscount();
            }
            System.out.printLn(product.getDiscount());
        }
    }

}

This is not a good desing. It is agains LSP. We should have been able to deal with all the objects as Product objects itself instead of typecasting to InHouseProduct.

public class Product {

    protected double discount;

    public double getDiscount() {
        return discount;
    };

}

public class InHouseProduct extends Product
{
    @Override
    public double getDiscount() {
        this.applyExtraDiscount();
        return discount;
    }

    public void applyExtraDiscount() {
        discount = discount * 1.5;
    }
}

public class PricingUtils {

    public static void main(String[] args) {
        
        Product p1 = new Product();
        Product p2 = new Product();
        Product p3 = new InHouseProduct();

        List<Product> productList = new ArrayList<>();

        productList.add(p1);
        productList.add(p2);
        productList.add(p3);

        for (Product product : productList) {
            System.out.printLn(product.getDiscount());
        }
    }

}

### Live coding session

//Product.java
public class Product {

    protected double discount = 20;

    public double getDiscount() {
        return discount;
    };

}

//InHouseProduct
public class InHouseProduct extends Product
{
    @Override
    public double getDiscount() {
        this.applyExtraDiscount();
        return discount;
    }

    public void applyExtraDiscount() {
        discount = discount * 1.5;
    }
}

//PricingUtils.Java
import java.util.Arraylist;

public class PricingUtils {

    public static void main(String[] args) {
        
        Product p1 = new Product();
        Product p2 = new Product();
        Product p3 = new InHouseProduct();

        List<Product> productList = new ArrayList<>();

        productList.add(p1);
        productList.add(p2);
        productList.add(p3);

        for (Product product : productList) {
            System.out.printLn(product.getDiscount());
        }
    }

}

## Interface Segregation Principle
_Interface Segregation Principle (ISP)_: No client should be forced to depend on methods it does not use.

public interface IMultiFunction {

    public void print();
    public void getPrintSpoolDetails();
    public void scan();
    public void scanPhoto();
    public void fax();
    public void internetFax();

}

This interface defines methods that represent the various functions.

public class XeroxWokCentre implements IMultiFunction
{

    @Override
    public void print() {
        // printing code
    }

    @Override
    public void getPrintSpoolDetails {
        //  printing spool detail code
    }

    @Override
    public void scan {
        // scan code
    }

    @Override
    public void scanPhoto {
        // photo scan code
    }

    @Override
    public void fax {
        // fax code
    }

    @Override
    public void internetFax {
        // internet fax code
    }

}

This class represents a particular Xerox WorkCentre. Now, let's create an instance for a printer + scanner device.

public class HPPrinterNScanner implements IMultiFunction
{

    @Override
    public void print() {
        // print code
    }

    @Override
    public void getPrintSpoolDetails {
        // print spool detail code
    }

    @Override
    public void scan {
        // scan code
    }

    @Override
    public void scanPhoto {
        // scan photo code
    }

    @Override
    public void fax {
    
    }

    @Override
    public void internetFax {
        
    }

}

fax and internetFax are left unimplemented! We now have to creater an instance for a printer

public class CanonPrinter implements IMultiFunction
{

    @Override
    public void print() {
        // print code
    }

    @Override
    public void getPrintSpoolDetails {
        // print spool details code
    }

    @Override
    public void scan {

    }

    @Override
    public void scanPhoto {

    }

    @Override
    public void fax {
    
    }

    @Override
    public void internetFax {
        
    }

}

Everything except printing methods are left blank. Unimplemented methods are almost always indicative of a poor design. This goes against the ISP.

## Restructuring the code to follow ISP


public interface IMultiFunction {

    public void print();
    public void getPrintSpoolDetails();
    public void scan();
    public void scanPhoto();
    public void fax();
    public void internetFax();

}

public interface IPrint {

    public void print();

    public void getPrintSpoolDetails();

}

public interface IScan {
    
    public void scan();

    public void scanPhoto();

}

public interface IFax {

    public void fax ();

    public void internetFax();
}


public class XeroxWokCentre implements IPrint, IScan, Ifax
{

    @Override
    public void print() {
        // print code
    }

    @Override
    public void getPrintSpoolDetails {
        // print spool detail code
    }

    @Override
    public void scan {
        // scan code
    }

    @Override
    public void scanPhoto {
        // photo scan code
    }

    @Override
    public void fax {
        // fax code
    }

    @Override
    public void internetFax {
        // internet fax code
    }

}

public class HPPrinterNScanner implements Iprint, Iscan
{

    @Override
    public void print() {
        // print code
    }

    @Override
    public void getPrintSpoolDetails {
        // print spool detail code
    }

    @Override
    public void scan {
        // scan code
    }

    @Override
    public void scanPhoto {
        // photo scan code
    }
}

public class CanonPrinter implements Iprint
{

    @Override
    public void print() {
        // print code
    }

    @Override
    public void getPrintSpoolDetails {
        // print spool detail code
    }

}

The classes have become much cleaner now, no more blank implementations, thus avoiding ambiguity.

## Techniques to identify violations

1. Fat interfaces. Fat interfaces mean interfaces with high number of methods.
2. Interfaces with low cohesion. 
3. Empty implementations of methods.


# Dependency Inversion Principle

_Depenedency Inversion Peinciple (DIP)_: 
    High-level modules should not depend on low-level modules. Both should depend on abstractions.
    Abstractions should not depend on details. Details hsould depend on abstractions.


        ---------------------
        |   ProductCatalog  |
        ---------------------
                  |
                  |
                  |
                  v
      ---------------------------
      |   SQLProductRepository  |
      ---------------------------

// ProductCatalog
import java.util.List;

public class ProductCatalog {

    public void listAllProducts() {

        SqlProductRepository sqlProductRepository = new SqlProductRepository();

        List<String> allProductNames = sqlProductRepository.getAllProductNames();

        // Display product names

    }

}

// SQLProductRepository
import java.util.Arrays;
import java.util.List;

public class SQLProductRepository {

    public List<String> getAllProductNames() {
        return Arrays.asList("soap", "toothpaste");
    }

}

ProductCatalog directly depends on SQLProductRepository, so this is the fiolation of the principle as seen in code.

// ProductRepository
imnport java.util.List;

public interface ProductRepository {

    public List<String> getAllProductNames();

}

// ProductFactory
public class ProductFactory {

    public static ProductRepository create() {
        return new SQLProductRepository()
    }

}


// ProductCatalog
import java.util.List;

public class ProductCatalog {

    public void listAllProducts() {

        ProductRepository productRepository = ProductFactory.create();

        List<String> allProductNames = productRepository.getAllProductNames();

        // Display product names

    }

}

// SQLProductRepository
import java.util.Arrays;
import java.util.List;

public class SQLProductRepository implemets ProductRepository {

    public List<String> getAllProductNames() {
        return Arrays.asList("soap", "toothpaste");
    }

}


        ---------------------
        |   ProductCatalog  |
        ---------------------
                  |
                  |
                  v
       -------------------------
       |   ProductRepository   |
       -------------------------
                  ^
                  |
                  |
      ---------------------------
      |   SQLProductRepository  |
      ---------------------------


## Dependency Injection
Ideally, we don't want the ProductCatalog to worry about how and when to trigger the instantiation. Let's provide the instatiated repository class to ProductCatalog.

// ECommerceMainApplication {

    public static void main(Sring[] args) {

        ProductRepository productRepository = ProductFactory.create();
        ProductFactory.create();

        ProductCatalog productCatalog = new ProductCatalog(productRepository);

        productCatalog.listAllProducts;

    }

}

// ProductRepository
imnport java.util.List;

public interface ProductRepository {

    public List<String> getAllProductNames();

}

// ProductFactory
public class ProductFactory {

    public static ProductRepository create() {
        return new SQLProductRepository()
    }

}


// ProductCatalog
import java.util.List;

public class ProductCatalog {

    private ProductRepository productRepository;

    public ProductCatalog(ProductRepository productRepository) {
        this.productRepository = productRepository;
    }


    public void listAllProducts() {
        List<String> allProductNames = productRepository.getAllProductNames();
        
        // Display product names

    }

}

// SQLProductRepository
import java.util.Arrays;
import java.util.List;

public class SQLProductRepository implemets ProductRepository {

    public List<String> getAllProductNames() {
        return Arrays.asList("soap", "toothpaste");
    }

}

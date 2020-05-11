---
layout: post
title:  OOD-Design
categories: Interview
---

## Design a caller System
https://github.com/donnemartin/system-design-primer#object-oriented-design-interview-questions-with-solutions

System Admin: edit employee
Employee: has an ID, the contact info, the escalate method
Can we assume all input are valid？
Can we assume operators handle initial calls. If operator can't handle it. we escalete
, follow this rule to escalate.

All calls can be handled by director.
If not, we can just throw an exception

What if we don't have available people to handle the call?
Queue this call.

```java
enum CallStatus{
   INPROGRESS,
   READY,
   COMPLETE
}

enum Rank{
   Operator,
   Supervisor,
   Manager
}

class Call{
   Employee employee;
   CallStatus status;
   Phone phone;
   public Call(Phone phone){
      this.status = READY
      this.phone = phone;
   }
}

abstract class Employee{
   Stirng id;
   String Name;
   String Phone;
   String Addr;
   Rank rank;
   Call call;
   CallCenter callCenter;
   public Employee(String id,..){
      this.id = id;
      ...
   }
   public void escalate(){
      this.callCenter.escalateRequest(this.Rank,call);
      this.call.status = Ready
      this.call = null;
   }
   public void takeCall(Call call){
      this.call = call;
      this.call.employee = this;
      this.call.state = IN_PROGRESS;
   }
   public void finishCall(){
      this.call.status = COMPLETE;
      this.callCenter.callComplete(this)
      this.call = null;
   }
}

class Operator{
   @Override
   
}


class Supervisor{
   @Override
   public void escalate(){
      this.callCenter.escalateRequest(RANK.Supervisor,call);
      this.call.status = Ready
      this.call = null;
   }
}


   class Manager{
      @Override
      public void escalate(){
         throw new EscalationException("Not Implemented");
      }
   }

   class CallCenter{
      String ID;
      List<Employee> Operators;
      List<Employee> Supervisors;
      List<Employee> Managers;
      Queue<call> callQueue;
      public CallCenter(String ID){
         this.id = id;
      }
      public addEmployee(Employee e){
         if(e.rank == RANK.Operator){
            Operators.add(e);
         }
         else if(e.rank == RANK.Supervisor){
            ..
         }
         else if(e.rank == RANK.manager){
            ..
         }
         else{
            Throw new EmployeeException("Employee rank not implemented");
         }
      }
      public handleCall(Call call,RANK){
         if (Operators.size() > 0){
            Operator operator = Operators.pop_front();
            operator.handleCall(call);
         }
         else if(){

         }
         else if{

         }
         else{
            callQueue.add(call);
         }
      }
      public completeCall(Employee e){
         addEmployee(e);
         if(!callQueue.empty()){
            Call nextCall = callQueue.pop();
            handleCall(nextCall);
         }
      }
      public escalate(Rank rank, Call call){
         handleCall(call,rank);
      }
   }

```

## Design a Library Management System
Library Management:
Student, Member
Admin Edit books, add books

Barcode for each books and member 

Workflow:
1. Rent a book
scan membership card - scan book - check if book reference only
- check if book reserved - check if limit reached 
- rent book transaction
- book rent

2. Return a book
Scan membership card - scan book - check if pass due
- if pass due, calculate the amount of payment, payment transaction
- otherwise return the book. check if someone is trying to reserve this book.

```java
enum rentBookStatus{
    ..
    ..
}

enum BookStatus{
    Ready,
    Borrowed,
    Reserved,
    Wanted
}

class Book{
    Barcode barcode;
    Name name;
    Title title;
    Author author;
    BookStatus status;
    Boolean isReferenceOnly;
    BorrowedDate = null;
    Member reservedBy;
    Book(..){
        ...
        status = BookStatus.Ready;
    }
}

class Member{
    Barcode barcode;
    Email email;
    Name name;
    Phone phone;
    int numRented;
    public Member(..){
        numRented = 0
    }
    HashMap<Barcode,Book> borrowedBook;
    void BorrowBook(Book book,Date date){
        borrowedBook.add(book)
        book.status = Borrowed;
        book.borrowedDate = date;
    }
    // return number of payment
    int returnBook(BarCode barcode){
        if(borrowedBook.contains(barcode) == false){
            throw new InvalidOperationExcetpion("Book not borrowed by this member");
        }
        Book ret = borrowedBook.get(barcode);
        int payValue = 0;
        if(pass due){
            payValue = Caculate number;
        }
        book.status = Ready;
        book.borrowedDate = null;
        return payValue;
    }
}


Class LibrarySystem{
    Zipcode zipcode;
    List<Book> books;
    List<Member> members;

    List<Book> searchBooks(Title title){
        List<Book> ret = new ArrayList<>();
        for(all books){
            if(book.getTitle() == title){
                ret.add(book);
            } 
        }
        return ret
    }

    List<Book> searchBooks(Author...){
        
    }

    rentBookStatus rentBook(Member member, Book book){
        if(book.getReferenceOnly()){
            ...
            return new rentBookStatus(BookReferenceOnly);
        }
        if(book.getStatus)
            
            ..
            return new rentBookStatus(BookREservedByOthers);
        }
        if(!member.canRent()){
            ...
            return new rentBookStatus(MembershipIssue);
        }
        if bookStatus==reserved and reservedBy == self{
               
         }
        member.borrowBook(book);
        return new rentBookStatus(Successfull)
    }
    returnBookStatus returnBook(Member member,Book book){

    }
    reserveBookStatus reserveBook(Member member, Book book){

    }
}

```

## Design a parking lot
Two things

Can I assume there are 3 types of cars?
Motorcycle, normal cars, Large cars

User:
Phone:


Cars：
license plate



Parking space:
have a ID
have a hourly rate




Cars(Which is of course driven by a driver)
Space
```java
enum CarType{
   Motorcycle,
   Car,
   LargeCar
}

enum SpotType{
   Small,
   Normal,
   Large
}

Class Vehicle{
   String plate;
   CarType type;
 
   boolean canPark(ParkingSpot spot){

   }
}

class Motorcycle{
   public Motorcycle(..){
      super(..)
   }
   boolean canPark(ParkingSpot spot){
      return True
   }
}

class Car{
   public Car(..){
      super(..)
   }
   boolean canPark(ParkingSpot spot){
      if(spot.type != SpotType.small){
         return True;
      }
      return True;
   }
}

class BigCar{
   public BigCar(..){
      super(..);
   }
   boolean canPark(ParkingSpot spot){
      if (spot.type != SpotType.large){
         return False;
      }
      return True
   }
}


Class ParkingSpot{
   SpotType type;
   String id;
   boolean isAvaialble;
   Vehecle vehicle;
   public ParkingSpot(..){
      ..
   }

   public boolean isAvailable(){
      return this.isAvaialble;
   }

   public boolean canPark(Vehicle vehicle){
      if(isAvaialble ==false){
         return false;
      }
      return vehicle.canPark(this);
   }

   public boolean park(Vehicle vehicle){
      this.isAvailable = False;
      this.vehecle = vehicle
   }

   public boolean clearSpot(){
      this.isAvailable = True;
      this.vehicle = null;
   }
}

class ParkingLot{
   Zipcode zipcode;
   String Name;
   List<ParkingSpot> parkingSpotList;
   HashMap<Vehicle,ParkingSpot> VPMap;
   public void addParkingSpot(ParkingSpot pk){
      ps.add(pk);
   }
   public boolean park(Vehicle vehicle ){
      for(ParkingSpot ps : parkingSpotList){
         if(ps.canPark(vehicle)){
            vpMap.put(vehicle,ps);
            ps.parVehicle(vehicle);
            return false;
         }
      }
      return false;
   }
   public boolean clearSpot(Vehicle vehicle){
      if(VPmap.contains(vehicle) == null){
         //TODO:raise exception
      }
      VPMap.get(vehicle).clearSpot();
   }
}
```

## Design an elevator System
What user do:

Click buttons

Type of buttons:
1. Floor buttons
2. Emergency Button
3. Close, open the door





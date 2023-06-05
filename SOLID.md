# SOLID

The **SOLID principles** are a set of rules and best practices to follow while designing a class structure.

[[_TOC_]]

## **S**ingle Responsibility Principle

The **Single Responsibility Principle** suggests that a class, module, or function should have a _single_ responsibility/purpose in an application $^1$. According to one commonly used description of the principle, "[a] class should have one, and only one, reason to change."

**Example (C#):**

    public class Customer {

        public void RecalculatePremium()
        {
            ...
        }

        public void SendEmail(string message)
        {
            ...
        }

    }

According to the single responsibility principle, this class should not be responsible for both recalculating the customer's premium and sending an e-mail to the customer as it limits re-use and makes maintaining the application more difficult.

## **O**pen-closed Principle

According to the **Open-closed Principle**, classes should be open for extension (i.e. adding new functionalities), but closed for modification $^2$. In other words, we should be able to add new functionality without modifying the existing code and potentially introducing new defects to it.

The Open-closed Principle can take a number of forms depending upon the programming language $^3$:

- Function parameters
- Extension methods
- Abstract classes
- Interfaces
- Generics

In the below example (C#), the Open-closed Principle is demonstrated using inheritance with the virtual/override keywords.
public class CustomerSale {

        public IList<CustomerItems> Items { get; set; }

        public virtual double GetTotal() {
            double total;

            foreach(var item in Items){
                total += item.unitCost;
            }

            return total;
       }
    }

    public class DiscountedCustomerSale : CustomerSale {

        public double DiscountFactor { get; set;}

        public override double GetTotal() {
            double total;

            foreach(var item in items) {
                total += item.unitCost * (1 - DiscountFactor)
            }

            return total;
        }
    }

In this case, the DiscountCustomerSale class extends the CustomerSale class and provides its own implementation of the **GetTotal** method without altering the original type.

# **L**iskov Substitution Principle

The **Liskov Substitution Principle** states that subclasses should be substitutable for their base classes $^4$.

The classic example (C#) is shown below:

    public class Rectangle {
        private int width;
        private int height;

        // Post-condition: height has not changed
        public virtual void SetWidth(int value) {
        width = value;
        }

        // Post-condition: width has not changed
        public virtual void SetHeight(int value) {
        height = value;
        }

    }

    public class Square : Rectangle {

        public override void SetWidth(int value) {
            width = value;
            height = value;
        }

        public override void SetHeight(int value) {
            width = value;
            height = value;
        }

    }

In the above example, when the Square class overrides the implementation of _SetWidth_ and _SetHeight_ on the base class, it violates the Liskov Substitution Principle as the overridden methods are modifying both values, in violation of the base class's code contract.

In general, overridden types should always:

- Have the same signature as in the base class.
- Satisfy any code contracts associated with the base class implementation.
- Perform validation that is equally or less restrictive than that in the base class implementation.

# **I**nterface Segregation Principle

The **Interface Segregation Principle** states that clients should not be required to implement members of interfaces that they do not use $^5$.

In the below example (C#), the Interface Segregation Principle is violated:

    public interface ICollection {
        public Item GetItem(int index);
        public void AddItem(Item item);
    }

    public class ArrayCollection : ICollection {

        public Item GetItem(int index) {
            ...
        }

        public void AddItem(item item) {
            ...
        }

    }

    public class ReadOnlyCollection : ICollection {

        public Item GetItem(int index) {
            ...
        }

        public void AddItem(Item item) {
            throw new NotSupportedException();
        }

    }

The class ReadOnlyCollection implements the ICollection interface, but the _AddItem_ method is not applicable to that type of collection. In this case, the interface should be segregated such that the class can implement only the members it needs:

    public interface ICollection {
        public Item GetItem(int index);
    }

    public interface IModifiableCollection {
        public void AddItem(Item item)
    }

    public class ArrayCollection : ICollection, IModifiableCollection {

        public Item GetItem(int index) {
            ...
        }

        public void AddItem(item item) {
            ...
        }

    }

    public class ReadOnlyCollection : ICollection {

        public Item GetItem(int index) {
            ...
        }

    }

# Dependency Inversion Principle

The **Dependency Inversion Principle** states that classes should depend upon interfaces or abstract classes rather than concrete classes and functions $^6$.

In the below example, the Enrollment class, is tightly coupled with the HttpService class:

    public class HttpService {

        public string GetPostResponse(string requestBody) {
            ...
        }

    }

    public class EnrollmentProcessing {

        private readonly HttpService httpService;

        public EnrollmentProcessing(HttpService httpService) {
            this.httpService = httpService;
        }

        public void ProcessEnrollment(EnrollmentApplication enrollmentApplication) {
            ...
            var response = HttpService.GetPostResponse(enrollmentApplication.ApplicationContents);
        }

    }

This tight coupling is problematic for unit testing (as we are unable to provide a "mock" implementation of the service) and doesn't allow for extensibility as modifying the original class (to allow for other implementations of the HttpService) would also violate the open-closed principle.

To re-factor the above example, the HttpService is abstracted:

        public interface IHttpService {
            string GetPostResponse(string requestBody);
        }

        public class HttpService : IHttpService {

            public string GetPostResponse(string requestBody) {
                ...
            }
        }

        public class EnrollmentProcessing {

            private readonly IHttpService httpService;

            public EnrollmentProcessing(IHttpService httpService) {
                this.httpService = httpService;
            }

            public void ProcessEnrollment(EnrollmentApplication enrollmentApplication) {
                ...
                var response = HttpService.GetPostResponse(enrollmentApplication.ApplicationContents);
            }

        }

# References

$^1$ "[SOLID Design Principles Explained: The Single Responsibility Principle](https://stackify.com/solid-design-principles/)," _Stackify_. Retrieved 05 June 2023.  
$^2$ "[SOLID Design Principles Explained: The Open/Closed Principle with Code Examples](https://stackify.com/solid-design-open-closed-principle/)," _Stackify_. Retrieved 05 June 2023.  
$^3$ "[SOLID: Open-Closed Principle](https://www.tutorialsteacher.com/csharp/open-closed-principle)," _TutorialsTeacher_. Retrieved 05 June 2023.  
$^4$ "[SOLID Design Principles Explained: The Liskov Substitution Principle with Code Examples](https://stackify.com/solid-design-liskov-substitution-principle/)," _Stackify_. Retrieved 05 June 2023.  
$^5$ "[SOLID Design Principles Explained: Interface Segregation with Code Examples](https://stackify.com/interface-segregation-principle/)," _Stackify_. Retrieved 05 June 2023.  
$^6$ "[SOLID: The First 5 Principles of Object Oriented Design](https://www.digitalocean.com/community/conceptual-articles/s-o-l-i-d-the-first-five-principles-of-object-oriented-design#dependency-inversion-principle)," _Digital Ocean_. Retrieved 05 June 2023.

# SOLID

The **SOLID principle** is a set of rules and best practices to follow while designing a class structure.

## **S**ingle Responsibility Principle

The **single responsibility principle** suggests that a class, module, or function should have a _single_ responsibility/purpose in an application. According to one commonly used definition, "every class should have only one reason to change."

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

According to the **open-closed principle**, classes should be open for extension (i.e. adding new functionalities), but closed for modification. In other words, we should be able to add new functionality without modifying the existing code and potentially introducing new bugs to it.

The open-closed principle can take a number of forms:

- Function parameters
- Extension methods
- Abstract classes
- Interfaces
- Generics

In the below example (C#), the open-closed principle is demonstrated using inheritance with the virtual/override keywords.
public class CustomerSale {

        public IList<CustomerItems> Items { get; set; }


        public virtual double GetTotal() {
            double total;

            foreach(var item in Items){
                total += item.unitCost;
            }
       }
    }


    public class DiscountedCustomerSale : CustomerSale {

        public double DiscountFactor { get; set;}

        public override double GetTotal() {
            double total;

            foreach(var item in items) {
                total += item.unitCost * (1 - discountFactor)
            }
        }
    }

In this case, the DiscountCustomerSale class extends the CustomerSale class and provides its own implementation of the **GetTotal** method without altering the original type.

# **L**iskov Substitution Principle

The **Liskov Substitution Principle** states that subclasses should be substitutable for their base classses.

In the below example (C#), the MedicareMember type should be able to be substituted for its base class (Member) in the _ProcessEnrollment_ method:

    public class Member {
        ...
    }

    public class MedicareMember: Member {
        ...
    }

    public class EnrollmentManager {
        public void ProcessEnrollment(Member member)
    }

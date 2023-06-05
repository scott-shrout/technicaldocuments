# SOLID

The **SOLID principle** is a set of rules and best practices to follow while designing a class structure.

## **S**ingle Responsibility Principle

The **single responsibility principle** suggests that a class, module, or function should have a _single_ responsibility/purpose in an application. According to one commonly used definition, "every class should have only one reason to change."

**Example:**

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
-

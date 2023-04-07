# Write a C# WinForms application that allows users to create a new customer record and save it to a database. The customer record should include a name, email address, and phone number.

1. Create a new WinForms project in Visual Studio.

2. Add TextBox controls for the name, email address, and phone number fields to the form.

3. Add a Save button to the form.

4. In the form's Load event, initialize a new instance of the Entity Framework DbContext to connect to the database.

5. In the Save button's Click event, write the code to create a new Customer object, set its properties based on the values entered in the TextBox controls, and add it to the DbContext. Here is an example code snippet:

```csharp
using (var db = new YourDbContext())
{
    var customer = new Customer
    {
        Name = nameTextBox.Text,
        Email = emailTextBox.Text,
        Phone = phoneTextBox.Text
    };
    db.Customers.Add(customer);
    db.SaveChanges();
}
```

6. When the user clicks the Save button, call the code to save the new customer record to the database.

7. Add validation code to ensure that the name field is not empty, and that the email and phone fields are valid. Display an error message to the user if any validation errors are found.

8. Run the application and test the functionality by creating and saving a new customer record.
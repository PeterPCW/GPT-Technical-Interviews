# Write a C# WinForms application that displays a list of customers from a database. Include a search bar that allows users to filter the list by customer name.

1. Create a new WinForms project in Visual Studio.

2. Add a DataGridView control to the form by dragging and dropping it from the toolbox.

3. Add a TextBox control and a Label control to the form to create the search bar.

4. In the form's Load event, write the code to populate the DataGridView control with data from the database. Here is an example code snippet that uses Entity Framework to retrieve data from a database:

```csharp
using (var db = new YourDbContext())
{
    var customers = db.Customers.ToList();
    dataGridView1.DataSource = customers;
}
```

5. Add an event handler to the TextBox control's TextChanged event that filters the DataGridView control based on the customer name. Here is an example code snippet that filters the DataGridView control based on the customer's name:

```csharp
private void textBox1_TextChanged(object sender, EventArgs e)
{
    string searchTerm = textBox1.Text.ToLower();
    dataGridView1.ClearSelection();
    foreach (DataGridViewRow row in dataGridView1.Rows)
    {
        if (row.Cells["Name"].Value.ToString().ToLower().Contains(searchTerm))
        {
            row.Visible = true;
        }
        else
        {
            row.Visible = false;
        }
    }
}
```

6. Run the application and test the search functionality by entering a search term in the TextBox control.
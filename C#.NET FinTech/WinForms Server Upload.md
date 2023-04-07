# Write a C# WinForms application that allows users to upload a file to a server. The application should display the progress of the upload in real-time.

1. Create a new WinForms project in Visual Studio.

2. Add a Button control to the form for users to initiate the file upload.

3. Add a ProgressBar control to the form to display the upload progress.

4. In the Button control's Click event, open a FileDialog to allow the user to select the file to upload.

5. Use the WebClient class to upload the file to the server. You can handle the UploadProgressChanged event of the WebClient to update the ProgressBar control in real-time. Here is an example code snippet:

```csharp
using (var client = new WebClient())
{
    client.UploadProgressChanged += (sender, e) =>
    {
        progressBar.Value = e.ProgressPercentage;
    };
    var fileData = File.ReadAllBytes(filePath);
    client.UploadDataAsync(new Uri(uploadUrl), "POST", fileData);
}
```

6. When the file upload is complete, display a message to the user indicating that the upload was successful.

7. Add error handling code to display an error message to the user if the upload fails.

8. Run the application and test the functionality by selecting a file to upload and verifying that the progress bar updates in real-time.
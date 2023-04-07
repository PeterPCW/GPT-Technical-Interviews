# Write a C# method that takes a string and returns the number of vowels in the string.



```csharp
public static int CountVowels(string str)
{
    int count = 0;
    foreach (char c in str.ToLower())
    {
        if (c == 'a' || c == 'e' || c == 'i' || c == 'o' || c == 'u')
        {
            count++;
        }
    }
    return count;
}
```

This method uses a loop to iterate through each character in the input string, converts it to lowercase to simplify the comparison, and checks if the character is a vowel. If it is, the method increments the count variable. Finally, the method returns the count of vowels found in the string.

You can call this method from other parts of your code as follows:

```csharp
string str = "Hello World";
int vowelCount = CountVowels(str);
Console.WriteLine("The number of vowels in '{0}' is {1}", str, vowelCount);
```
# Write a C# method that takes an array of integers and returns the sum of all even numbers in the array.

```csharp
public static int SumEvenNumbers(int[] arr)
{
    int sum = 0;
    foreach (int num in arr)
    {
        if (num % 2 == 0)
        {
            sum += num;
        }
    }
    return sum;
}
```

This method takes an integer array as input and uses a loop to iterate through each number in the array. It checks if the number is even by using the modulus operator **`%`** to determine if the remainder when dividing by 2 is 0. If it is, the method adds the number to the **`sum`** variable. Finally, the method returns the sum of all even numbers found in the array.

You can call this method from other parts of your code as follows:

```csharp
int[] arr = { 1, 2, 3, 4, 5, 6 };
int sumEvenNumbers = SumEvenNumbers(arr);
Console.WriteLine("The sum of all even numbers in the array is {0}", sumEvenNumbers);
```
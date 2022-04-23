https://www.geeksforgeeks.org/what-is-array-decay-in-c-how-can-it-be-prevented/

# What is Array Decay? 
The loss of type and dimensions of an array is known as decay of an array.This generally occurs when we pass the array into function by value or pointer. What it does is, it sends first address to the array which is a pointer, hence the size of array is not the original one, but the one occupied by the pointer in the memory.

# How to prevent Array Decay? 
1. A typical solution to handle decay is to pass size of array also as a parameter and not use sizeof on array parameters (See this for details)
2. Another way to prevent array decay is to send the array into functions by reference. This prevents conversion of array into a pointer, hence prevents the decay.
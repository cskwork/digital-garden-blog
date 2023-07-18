# C Pointer Theory

A pointer in C is a variable that ***stores the memory address of another variable***. 
Pointers are useful for various purposes, such as dynamic memory allocation
, passing large data structures between functions more efficiently
, and implementing data structures like linked-lists, trees, and graphs.

To explain pointers, let's go through a simple example:

```c
#include <stdio.h>

int main() {
    int x = 42;       // Declare an integer variable x with value 42
    int *pointer_to_x; // Declare a pointer to an integer variable

    pointer_to_x = &x; // Assign the address of x to the pointer_to_x

    printf("x = %d\n", x);                  // Print the value of x
    printf("Address of x: %p\n", &x);       // Print the address of x
	
	// Print the ***value stored in pointer_to_x*** (which is the address of x)
    printf("pointer_to_x = %p\n", pointer_to_x); 
	
	// *Dereference the pointer and ***print the value*** it points to (which is x)
    printf("Value pointed by pointer_to_x: %d\n", *pointer_to_x); 
    return 0;
}
```

Here's a breakdown of what happens in the code:

1. We create an integer variable called "x" and assign the value 42 to it.
2. We declare a pointer to an integer called "pointer_to_x."
3. We store the address of "x" in "pointer_to_x" by using the address-of operator (&) before "x."
4. We print the value of "x," the address of "x," the value of "pointer_to_x" (which is the same as the address of "x"), and the value pointed to by "pointer_to_x" (which is the value of "x").

The key pointer operations are:

1. Assigning the address to a pointer using the & (ampersand) operator.
2. Dereferencing a pointer with the * (asterisk) operator to get the value it points to.

When working with pointers, be cautious about dereferencing uninitialized or NULL pointers
, as it can lead to undefined behavior and crashes.

## EXAMPLE 
- pointers to swap the values of two integer variables:

```c
#include <stdio.h>

// Function to swap two integers using pointers
void swap(int *a, int *b) {
    int temp = *a;
    *a = *b;
    *b = temp;
}

int main() {
    int x = 5;
    int y = 10;

    printf("Before swapping: x = %d, y = %d\n", x, y);

    swap(&x, &y); // Pass the addresses of x and y to the swap function

    printf("After swapping: x = %d, y = %d\n", x, y);

    return 0;
}
```
In this example, we have a function called swap that takes two integer pointers as arguments. 
Inside the swap function, we first store the value pointed to by the first pointer (*a) in a temporary variable temp. 
Then, we assign the value pointed to by the second pointer (*b) to the location pointed to by the first pointer (*a). 
Finally, we assign the value stored in temp to the location pointed to by the second pointer (*b). 
In the main function, we have two integer variables x and y. 
We print their values before and after calling the swap function. 
We pass the addresses of x and y to the swap function using the & operator. 
The result is that the values of x and y are swapped.

RESULT
```c
Before swapping: x = 5, y = 10
After swapping: x = 10, y = 5
```

# CHAR AND ARRAY POINTERS 
CAN: In C, char pointers and array pointers are similar in their behavior. 
A char pointer is used to store the memory address of a character variable
, while an array pointer stores the memory address of the first element of an array. 
Let's discuss char and array pointers separately with examples.

## 1. Char Pointers:

Char pointers are useful for working with strings since strings in C are arrays of characters terminated by a null character ('\0'). 

Here's a simple example showing how char pointers work:
```c
#include <stdio.h>

int main() {
    char ch = 'A';
	
	// Declare a character pointer and store the address of ch
    char *char_ptr = &ch; 

	// Dereference pointer to print the character
    printf("Character: %c\n", *char_ptr); 

    return 0;
}
```

## 2. Array Pointers:

Similar to a char pointer, an array pointer holds the address of the first element of an array. 
In C, when you use the name of an array without any index
, it's implicitly converted to a pointer to the first element. 
Here's an example showing how pointers and arrays are related:

```c
#include <stdio.h>

int main() {
    int arr[] = {10, 20, 30, 40, 50}; // Declare an array of integers
	
	// Declare a pointer to int and store the address of the first element of arr
    int *arr_ptr = arr;
	
	// Dereference the pointer to access the first element
    printf("First element of the array: %d\n", *arr_ptr); 
	
    return 0;
}
```

Note that when you use an array name without index, it acts as a pointer. 
In the example above, assigning `arr` to `arr_ptr` assigns the address of the first element of the array to the pointer.

You can also perform pointer arithmetic, iterate through arrays using pointers
, and access elements using pointer notation. Remember that accessing memory beyond the array bounds or dereferencing an uninitialized pointer can lead to undefined behavior.


## Char-Array Pointer 

```c
#include <stdio.h>

void main() {
    char *p = "KOREA";

    printf("%s\n", p);
    printf("%s\n", p + 3);
    printf("%c\n", *p);
    printf("%c\n", *(p + 3));
    printf("%c\n", *p + 2);
}
```

Now let's go through each line to understand the pointer operations:

1. `char *p = "KOREA";`

This line declares a char pointer `p` and initializes it with the address of the first character of the string "KOREA". 
In C, a string is an array of characters terminated by a null character ('\0').

2. `printf("%s\n", p);`

This line prints the string pointed to by `p`
, which will output "KOREA" because `p` points to the beginning of the string.

3. `printf("%s\n", p + 3);`

This line prints the string starting from the 4th character (0-indexed) of the original string. 
Pointer arithmetic is used here to add 3 to the base address (`p`); thus
, `p + 3` points to the 'E' character. The output will be "EA".

4. `printf("%c\n", *p);`

This line prints the character pointed to by `p`, which is the first character of the string "KOREA". 
The output will be 'K'.

5. `printf("%c\n", *(p + 3));`

This line prints the character located at the address `p + 3`. 
It first adds 3 to the base address (`p`) and then dereferences the resulting pointer (*(p+3)). 
The output will be 'E'.

6. `printf("%c\n", *p + 2);`

This line first dereferences `p` to get the 'K' character, then adds 2 to its ASCII value. 
Since the ASCII value of 'K' is 75, adding 2 results in the value 77, which corresponds to the character 'M'.
The output will be 'M'.

https://stackoverflow.com/questions/10186765/what-is-the-difference-between-char-array-and-char-pointer-in-c

## Two different uses of character string literals

1 Initialize char[]:

```c
char c[] = "abc";      
=
char c[] = {'a', 'b', 'c', '\0'};
```

2 Initialized with a character string literal.
- creates array of char (unable to modify) 

```c
char *c = "abc";
=
/* __unnamed is magic because modifying it gives UB(undefined behavior). */
static char __unnamed[] = "abc";
char *c = __unnamed;
```

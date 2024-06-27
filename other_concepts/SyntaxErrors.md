You cannot assign values to an array in Java using the syntax `ans={-1,0,0};` after it has been declared. This syntax is only allowed during the array initialization. Instead, you should either initialize the array with the values directly at the time of declaration or assign the values individually after declaring the array. Here’s how you can do it correctly:

### Initialization at Declaration
You can initialize the array with the values directly when you declare it:

```java
int[] ans = {-1, 0, 0};
```

### Separate Declaration and Assignment
If you declare the array first, you need to assign values to each element individually:

```java
int[] ans = new int[3];  // Declare the array with a size of 3
ans[0] = -1;
ans[1] = 0;
ans[2] = 0;
```

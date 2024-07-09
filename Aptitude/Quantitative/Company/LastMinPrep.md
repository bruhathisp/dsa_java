# diamond pattern

``` java

public class DiamondPattern {

    // Method to print the diamond pattern
    static void printDiamond(int n) {
        // Print the upper part of the diamond
        for (int i = 1; i <= n; i++) {
            // Print leading spaces
            for (int j = i; j < n; j++) {
                System.out.print(" ");
            }
            // Print asterisks
            for (int j = 1; j <= 2 * i - 1; j++) {
                System.out.print("*");
            }
            System.out.println();
        }
        
        // Print the lower part of the diamond
        for (int i = n - 1; i >= 1; i--) {
            // Print leading spaces
            for (int j = n; j > i; j--) {
                System.out.print(" ");
            }
            // Print asterisks
            for (int j = 1; j <= 2 * i - 1; j++) {
                System.out.print("*");
            }
            System.out.println();
        }
    }

    // Main method
    public static void main(String[] args) {
        int n = 5;
        printDiamond(n); // Print the diamond pattern
    }
}



```
``` 
    *
   ***
  *****
 *******
*********
 *******
  *****
   ***
    *

```

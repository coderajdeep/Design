**Fixed size `ArrayList` with given value in Java**
**Similar to `vector<int> vec(n, -1)` in C++**

```java
import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

public class Test {
    public static void main(String[] args) {
        int n = 5;
        // This will only create ArrayList with capacity 5
        // But Java does not provide any method to get capacity
        // List<Integer> numbers = new ArrayList<>(n);
        // This will create ArrayList of size n and value will be -1
        List<Integer> numbers = new ArrayList<>(Collections.nCopies(n, -1));
        System.out.println(numbers);
    }
}

// Another way
import java.util.ArrayList;
import java.util.Collections;

public class Main {
    public static void main(String[] args) {
        int n = 10; // Size of the ArrayList
        ArrayList<Integer> list = new ArrayList<>(n);
        
        // Initially populate the ArrayList with nulls
        for (int i = 0; i < n; i++) {
            list.add(null);
        }

        // Now fill the list with -1
        Collections.fill(list, -1);

        System.out.println(list); // Prints [-1, -1, -1, ..., -1]
    }
}

```
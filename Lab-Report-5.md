***Lab Report 5***

***Part 1) Debugging Scenario***

**1) Edstem Post**

*Title: Debugging Help Needed: `filter` Method Reverses List Order*

I've been working on an assignment that involves filtering a list of strings based on certain criteria using the given `filter` method. The method is supposed to return a new list containing only the elements that satisfy a condition defined by a `StringChecker` interface, while maintaining the original order of the list.

Here's part of the filter method for context:

```
class ListExamples {

  // Returns a new list that has all the elements of the input list for which
  // the StringChecker returns true, and not the elements that return false, in
  // the same order they appeared in the input list;
  static List<String> filter(List<String> list, StringChecker sc) {
    List<String> result = new ArrayList<>();
    for(String s: list) {
      if(sc.checkString(s)) {
        result.add(0, s);
      }
    }
    return result;
  }
```
Input List: ["apple", "banana", "cherry", "date"]

Condition: String length greater than 5

Expected Output: ["banana", "cherry"]

Actual Output: ["cherry", "banana"]

The issue is the `filter` method is returning the filtered elements in reverse order. Based on my understanding, the output order should have been the same as the input list's order for the elements that satisfy the filtering condition.

I ran the test using `bash` on a `Tests.sh` file that saved the output to a file called `output.txt ` it used the`ListExamplesTest` class, which calls the filter method with a `StringChecker` implementation that checks for string lengths greater than 5. Instead of getting the expected output, the terminal output indicates that the list order was reversed during the filtering process. 


LabReportIMG2.2


Can anyone explain why the filter method reverses the order of the elements in the output list? Also, any suggestions on how to correct this behavior to maintain the original order of elements in the filtered list would be greatly appreciated!


**2) A response from a TA**

Based on your description and the code snippet you shared, the key to understanding why the elements are appearing in reverse order lies in this specific line of your `filter` method:
~~~
result.add(0, s); // Adding each string to the beginning of the list
~~~
By adding each matching string to the beginning of your results list `(result.add(0, s))`, you're essentially pushing previously added elements one position back in the list, which results in the reverse of the original order by the time you've iterated through the entire input list.

Suggestion:

To maintain the original order of elements that pass the StringChecker test, you should add them to the end of the result list instead. This can be achieved by simply using result.add(s) without specifying the index. This method appends the element to the end of the list, preserving the order.

**3)Student trying the TAs suggestion**

Line was changed from 
```
result.add(0, s); 
```
to
```
result.add(s);
```

New Test Case Output:

Condition: String length greater than 5
Expected Output: ["banana", "cherry"]
Actual Output after Correction: ["banana", "cherry"]

LabReport3IMG

Understanding the Bug:

The issue stemmed from how elements were being added to the results list. By using `result.add(0, s)`, each new element satisfying the condition was placed at the beginning of the list, pushing all previously added elements one position back. This resulted in the reverse order of the elements by the time the entire input list was processed.

By switching to `result.add(s)`, which appends elements to the end of the list, we maintain the original order of elements as they appear in the input list and satisfy the filtering condition.


**4)information needed about the setup**

File & Directory Structure:

ListExamples.java - Contains the original filter and merge methods.
ListExamplesTest.java - Contains the test case for the filter method.
runTests.sh (or runTests.bat for Windows) - Shell script (or batch file for Windows) to compile and run ListExamplesTest and redirect output to output.txt.

Contents of Each File Before Fixing the Bug:

ListExamples.java:

```
import java.util.ArrayList;
import java.util.List;

interface StringChecker { boolean checkString(String s); }

class ListExamples {

  static List<String> filter(List<String> list, StringChecker sc) {
    List<String> result = new ArrayList<>();
    for(String s: list) {
      if(sc.checkString(s)) {
        result.add(0,s);
      }
    }
    return result;
  }


  // Takes two sorted list of strings (so "a" appears before "b" and so on),
  // and return a new list that has all the strings in both list in sorted order.
  static List<String> merge(List<String> list1, List<String> list2) {
    List<String> result = new ArrayList<>();
    int index1 = 0, index2 = 0;
    while(index1 < list1.size() && index2 < list2.size()) {
      if(list1.get(index1).compareTo(list2.get(index2)) < 0) {
        result.add(list1.get(index1));
        index1 += 1;
      }
      else {
        result.add(list2.get(index2));
        index2 += 1;
      }
    }
    while(index1 < list1.size()) {
      result.add(list1.get(index1));
      index1 += 1;
    }
    while(index2 < list2.size()) {
      result.add(list2.get(index2));
      index1 += 1;
    }
    return result;
  }
}
```

ListExamplesTest.java: 

```
import java.util.Arrays;
import java.util.List;

public class ListExamplesTest {

    public static void main(String[] args) {
        testFilter();
        testMerge();
    }

    private static void testFilter() {
        // Example setup for filter test
        List<String> list = Arrays.asList("apple", "banana", "cherry", "date");
        StringChecker lengthChecker = new StringChecker() {
            @Override
            public boolean checkString(String s) {
                return s.length() > 5;
            }
        };

        List<String> filteredList = ListExamples.filter(list, lengthChecker);
        System.out.println("Filtered List: " + filteredList);
    }

    private static void testMerge() {
        // Example setup for merge test
        List<String> list1 = Arrays.asList("apple", "cherry");
        List<String> list2 = Arrays.asList("banana", "date", "fig");

        List<String> mergedList = ListExamples.merge(list1, list2);
        System.out.println("Merged List: " + mergedList);
    }
}
```

runTests.sh:

```
#!/bin/bash
javac ListExamples.java ListExamplesTest.java
java ListExamplesTest > output.txt
```

The full command line (or lines) you ran to trigger the bug:
`bash Tests.sh`

A description of what to edit to fix the bug:
Changed the line:

```
result.add(0, s);
```
to 

```
result.add(s);
```

***Part 2 – Reflection***

During the second half of this quarter, I had a fun experience learning about `vim`, an incredibly powerful text editor that runs in the terminal. Initially, I found its mode-based editing — where you switch between different modes for inserting text and executing commands — to be quite challenging. However, as I delved deeper, I discovered a level of efficiency and control I hadn't experienced with more graphical text editors. The ability to navigate and edit files quickly, without leaving the keyboard, was a game-changer for me, especially during long coding sessions.

Another revelation was the use of `jdb`, the Java debugger, which opened up a new world of troubleshooting for me. Before, I'd rely heavily on print statements to understand what my code was doing, which was often inefficient. Learning to set breakpoints, step through code, and inspect variables with jdb provided me with a much clearer insight into the execution flow and the state of my application, allowing me to identify and fix bugs much more effectively.

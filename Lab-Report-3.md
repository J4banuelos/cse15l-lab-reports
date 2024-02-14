***Lab Report 3 (Week 5)***

**Bugs**

*A failure-inducing input for the buggy program*

`
@Test
public void testReverseInPlaceFailure() {
    int[] input = {1, 2, 3, 4};
    ArrayExamples.reverseInPlace(input);
    assertArrayEquals(new int[]{4, 3, 2, 1}, input);
}
`


*An input that doesn't induce a failure, but has a bug*
`
@Test
public void testReverseInPlaceFlawed() {
  int[] input = {1, 2, 3, 4};
  int[] supposedExpected = {1, 2, 3, 4}; 
  ArrayExamples.reverseInPlace(supposedExpected); 
  ArrayExamples.reverseInPlace(input);
  assertArrayEquals(supposedExpected, input); 
}
`
Why this test case is flawed:This test case is flawed because it uses the `reverseInPlace` method to generate the expected outcome (`supposedExpected`). If `reverseInPlace` does not work correctly, both input and `supposedExpecte`d will be incorrectly processed in the same way, making them equal by mistake and causing the test to pass.



*The symptom, as the output of running the tests*

![Image]LabReport3.1.png

*The bug, as the before*
`
public class ArrayExamples {
    public static void reverseInPlace(int[] array) {
        for(int i = 0; i < array.length / 2; i++) {
            int temp = array[i];
            array[i] = array[array.length - i];
            array[array.length - i] = temp;
        }
    }
}
`

*The Code Change Made after*
`
public class ArrayExamples {
    public static void reverseInPlace(int[] array) {
        for(int i = 0; i < array.length / 2; i++) {
            int temp = array[i];
            array[i] = array[array.length - i - 1]; // Subtract 1 to correct the index
            array[array.length - i - 1] = temp;
        }
    }
}
`
*Why this change works:* The fix addresses the issue by correctly calculating the index of the element to swap with. In the original code, the index `array.length - i` is out of bounds when `i` is `0`. By adjusting the index to `array.length - i - 1`, we ensure we're accessing valid indices within the array, thereby correctly reversing the array in place.This change ensures that the first element swaps with the last, the second with the second-last, and so on, correctly reversing the array.









*Ciation for my sources*
Find Command in Linux (Find Files and Directories): [Link](https://linuxize.com/post/how-to-find-files-in-linux-using-the-command-line/)

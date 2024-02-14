***Lab Report 3 (Week 5)***

**Bugs**

*A failure-inducing input for the buggy program*
`@Test
public void testReverseInPlaceFailure() {
    int[] input = {1, 2, 3, 4};
    ArrayExamples.reverseInPlace(input);
    assertArrayEquals(new int[]{4, 3, 2, 1}, input);
}`

*An input that doesn't induce a failure, but has a bug*
`@Test
public void testReverseInPlaceFlawed() {
  int[] input = {1, 2, 3, 4};
  int[] supposedExpected = {1, 2, 3, 4}; 
  ArrayExamples.reverseInPlace(supposedExpected); 
  ArrayExamples.reverseInPlace(input);
  assertArrayEquals(supposedExpected, input); 
}`

*The symptom, as the output of running the tests*

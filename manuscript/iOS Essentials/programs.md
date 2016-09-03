Program to find the largest palindromic substring
```java
package com.rajabishek;

public class Main {
    
    public static void main(String[] args) {
        String input = "forgeeksskeegfor";
        System.out.println("The longest substring is: " + longestSubstring(input));
    }

    public static String longestSubstring(String input) {
        int length = input.length();
        char[] characters = input.toCharArray();
        int start = 0;
        int maxLength = 1;
        int low,high;
        for(int i=1;i<length;++i){
            //Find the even length palindromes with centers i and i-1
            low = i;
            high = i-1;
            while(low>=0 && high<length && characters[low] == characters[high]){
                int palindromeLength = high - low + 1;
                if(palindromeLength > maxLength) {
                    maxLength = palindromeLength;
                    start = low;
                }
                --low;
                ++high;
            }

            //Find the odd length palindromes with center i-1 and i+1
            low = i-1;
            high = i+1;
            while(low>=0 && high<length && characters[low] == characters[high]){
                int palindromeLength = high - low + 1;
                if(palindromeLength > maxLength) {
                    maxLength = palindromeLength;
                    start = low;
                }
                --low;
                ++high;
            }
        }

        StringBuilder builder = new StringBuilder();
        int last = start + maxLength - 1;
        for(int i=start;i<=last;++i) {
            builder.append(characters[i]);
        }
        return builder.toString();
    }
}
```
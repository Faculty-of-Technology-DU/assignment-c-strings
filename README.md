# **C Programming Assignments: Strings as Character Arrays**

The assignments are organized into two sections, progressing from implementing string operations manually to using the standard C string library.

| Section                          | Problem                                       | Concepts covered                    |
| -------------------------------- | --------------------------------------------- | ----------------------------------- |
| **A: Without&#x20;**`<string.h>` | 1. String Length and Character Classification | Character traversal, counting       |
|                                  | 2. Manual String Copy                         | Array indexing, null termination    |
|                                  | 3. Lexicographical String Comparison          | Character-by-character comparison   |
|                                  | 4. Manual String Concatenation                | Array manipulation, boundaries      |
|                                  | 5. String Reversal and Palindrome Detection   | Two-way traversal                   |
|                                  | 6. Substring Searching                        | Nested loops, pattern matching      |
|                                  | 7. Word Counting and Whitespace Normalization | In-place modification               |
| **B: With&#x20;**`<string.h>`    | 8. String Length Using Library Functions      | `strlen()`, `strcspn()`             |
|                                  | 9. Full Name Construction                     | `strcpy()`, `strcat()`              |
|                                  | 10. Alphabetical Sorting of Names             | `strcmp()`, `strcpy()`              |
|                                  | 11. Character and Substring Searching         | `strchr()`, `strrchr()`, `strstr()` |
|                                  | 12. Sentence Tokenization                     | `strtok()`                          |

**Important:** In Section A, no functions from `<string.h>` or `<ctype.h>` are used. Standard input/output functions such as `fgets()` and `printf()` are permitted. Each program is independent and can be saved as a separate `.c` file.

## **Section A — Strings Without String Library Functions**

### **Problem 1. Determine String Length and Classify Characters**

**Problem statement:** Write a C program that reads a string containing alphabets, digits, spaces, and special characters. Without using `strlen()` or any character classification library function, calculate the total number of characters in the string. Additionally, classify and count the number of alphabetic characters, numerical digits, whitespace characters (spaces and tabs), and special characters.

The program must traverse the character array manually until the null terminator `'\0'` is encountered. The terminating null character and the newline introduced by input must not be included in the character count.

**Sample input:** `Hello C 123!`

**Expected output:**

```
Length: 12
Alphabets: 6
Digits: 3
Whitespaces: 2
Special characters: 1
```

```

#include <stdio.h>

int main(void) {
    char text[100];
    int length = 0;
    int alphabets = 0;
    int digits = 0;
    int whitespaces = 0;
    int special_characters = 0;

    printf("Enter a string: ");

    if (fgets(text, sizeof(text), stdin) == NULL) {
        return 1;
    }

    while (text[length] != '\0' && text[length] != '\n') {
        char current = text[length];

        if ((current >= 'A' && current <= 'Z') ||
            (current >= 'a' && current <= 'z')) {
            alphabets++;
        } else if (current >= '0' && current <= '9') {
            digits++;
        } else if (current == ' ' || current == '\t') {
            whitespaces++;
        } else {
            special_characters++;
        }

        length++;
    }

    printf("Length: %d\n", length);
    printf("Alphabets: %d\n", alphabets);
    printf("Digits: %d\n", digits);
    printf("Whitespaces: %d\n", whitespaces);
    printf("Special characters: %d\n", special_characters);

    return 0;
}

```

### **Problem 2. Copy a String Without Using&#x20;**`strcpy()`

**Problem statement:** Write a C program that reads a string into a character array and copies its contents into another character array without using any string library function.

Copy each character individually using array indexing. Ensure that the destination array contains a correctly positioned null terminator. Print both the original string and the copied string to verify the operation.

The copying operation must not modify the original character array.

```

#include <stdio.h>

int main(void) {
    char source[100];
    char destination[100];
    int index = 0;

    printf("Enter a string: ");

    if (fgets(source, sizeof(source), stdin) == NULL) {
        return 1;
    }

    while (source[index] != '\0' && source[index] != '\n') {
        destination[index] = source[index];
        index++;
    }

    source[index] = '\0';
    destination[index] = '\0';

    printf("Original string: %s\n", source);
    printf("Copied string: %s\n", destination);

    return 0;
}

```

### **Problem 3. Compare Two Strings Lexicographically**

**Problem statement:** Write a C program to read two strings and compare them lexicographically without using `strcmp()` or any other string library function.

The comparison must be performed character by character, starting from the first character of each string. The program must determine whether the strings are identical, the first string is lexicographically smaller, or the first string is lexicographically greater.

If one string is a prefix of the other, the shorter string must be considered lexicographically smaller. The comparison must be case-sensitive.

For example, `Apple` is smaller than `Banana`, while `cat` is greater than `car`.

```

#include <stdio.h>

int main(void) {
    char first[100];
    char second[100];
    int index = 0;
    int length = 0;

    printf("Enter first string: ");

    if (fgets(first, sizeof(first), stdin) == NULL) {
        return 1;
    }

    printf("Enter second string: ");

    if (fgets(second, sizeof(second), stdin) == NULL) {
        return 1;
    }

    while (first[length] != '\0' && first[length] != '\n') {
        length++;
    }

    first[length] = '\0';
    length = 0;

    while (second[length] != '\0' && second[length] != '\n') {
        length++;
    }

    second[length] = '\0';

    while (first[index] != '\0' &&
           second[index] != '\0' &&
           first[index] == second[index]) {
        index++;
    }

    if (first[index] == second[index]) {
        printf("Both strings are identical.\n");
    } else if ((unsigned char)first[index] <
               (unsigned char)second[index]) {
        printf("First string is lexicographically smaller.\n");
    } else {
        printf("First string is lexicographically greater.\n");
    }

    return 0;
}

```

### **Problem 4. Concatenate Two Strings Manually**

**Problem statement:** Write a C program that reads two strings and combines them into a single string without using `strcat()`, `strcpy()`, or any other string library function.

The program must first copy the contents of the first string into a third character array, followed immediately by the contents of the second string. It must append the null terminator at the end of the resulting string.

The original strings must remain unchanged. Ensure that the destination array is large enough to accommodate the two strings and their terminating null character.

```

#include <stdio.h>

int main(void) {
    char first[100];
    char second[100];
    char combined[200];

    int index = 0;
    int position = 0;
    int length = 0;

    printf("Enter first string: ");

    if (fgets(first, sizeof(first), stdin) == NULL) {
        return 1;
    }

    printf("Enter second string: ");

    if (fgets(second, sizeof(second), stdin) == NULL) {
        return 1;
    }

    while (first[length] != '\0' && first[length] != '\n') {
        length++;
    }

    first[length] = '\0';
    length = 0;

    while (second[length] != '\0' && second[length] != '\n') {
        length++;
    }

    second[length] = '\0';

    while (first[index] != '\0') {
        combined[position] = first[index];
        position++;
        index++;
    }

    index = 0;

    while (second[index] != '\0') {
        combined[position] = second[index];
        position++;
        index++;
    }

    combined[position] = '\0';

    printf("Concatenated string: %s\n", combined);

    return 0;
}

```

### **Problem 5. Reverse a String and Check Whether It Is a Palindrome**

**Problem statement:** Write a C program that reads a string, calculates its length manually, and creates a reversed copy of the string without using any string library function.

Using the original and reversed character arrays, determine whether the entered string is a palindrome. A palindrome is a string that reads identically in both forward and reverse directions.

The comparison must be case-sensitive and must include all characters, including spaces and punctuation. For instance, `madam` is a palindrome, whereas `Madam` is not under this comparison rule.

```

#include <stdio.h>

int main(void) {
    char original[100];
    char reversed[100];

    int length = 0;
    int is_palindrome = 1;

    printf("Enter a string: ");

    if (fgets(original, sizeof(original), stdin) == NULL) {
        return 1;
    }

    while (original[length] != '\0' &&
           original[length] != '\n') {
        length++;
    }

    original[length] = '\0';

    for (int index = 0; index < length; index++) {
        reversed[index] = original[length - index - 1];
    }

    reversed[length] = '\0';

    for (int index = 0; index < length; index++) {
        if (original[index] != reversed[index]) {
            is_palindrome = 0;
            break;
        }
    }

    printf("Reversed string: %s\n", reversed);

    if (is_palindrome) {
        printf("The string is a palindrome.\n");
    } else {
        printf("The string is not a palindrome.\n");
    }

    return 0;
}

```

### **Problem 6. Find All Occurrences of a Substring**

**Problem statement:** Write a C program to read a main string and a smaller search string (substring). Without using `strstr()` or any other string library function, determine all starting positions at which the substring occurs within the main string.

The program must perform the search using nested loops and compare individual characters. Overlapping occurrences must also be identified.

For example, searching for `aba` in `ababa` must report two occurrences, starting at indices `0` and `2`. Use zero-based indexing and display the total number of matches.

```

#include <stdio.h>

int main(void) {
    char text[150];
    char pattern[50];

    int text_length = 0;
    int pattern_length = 0;
    int occurrences = 0;

    printf("Enter main string: ");

    if (fgets(text, sizeof(text), stdin) == NULL) {
        return 1;
    }

    printf("Enter substring: ");

    if (fgets(pattern, sizeof(pattern), stdin) == NULL) {
        return 1;
    }

    while (text[text_length] != '\0' &&
           text[text_length] != '\n') {
        text_length++;
    }

    text[text_length] = '\0';

    while (pattern[pattern_length] != '\0' &&
           pattern[pattern_length] != '\n') {
        pattern_length++;
    }

    pattern[pattern_length] = '\0';

    if (pattern_length == 0) {
        printf("Substring cannot be empty.\n");
        return 1;
    }

    printf("Matching positions: ");

    for (int index = 0;
         index <= text_length - pattern_length;
         index++) {

        int matched = 1;

        for (int offset = 0;
             offset < pattern_length;
             offset++) {

            if (text[index + offset] != pattern[offset]) {
                matched = 0;
                break;
            }
        }

        if (matched) {
            printf("%d ", index);
            occurrences++;
        }
    }

    if (occurrences == 0) {
        printf("None");
    }

    printf("\nTotal occurrences: %d\n", occurrences);

    return 0;
}

```

### **Problem 7. Count Words and Normalize Whitespace**

**Problem statement:** Write a C program that reads a complete sentence into a character array and performs two operations without using any string library function.

First, count the number of words in the sentence, considering any sequence of spaces or tab characters as a separator between words. Second, modify the same character array to eliminate leading and trailing whitespace and replace consecutive spaces or tabs with a single space.

The program must perform the normalization in place using separate reading and writing indices, without creating another character array. Display the normalized sentence and its word count.

**Sample input:** `   C    programming   is    interesting   `

**Expected output:**

```
Normalized sentence: C programming is interesting
Number of words: 4
```

```

#include <stdio.h>

int main(void) {
    char sentence[200];

    int read_index = 0;
    int write_index = 0;
    int word_count = 0;
    int inside_word = 0;

    printf("Enter a sentence: ");

    if (fgets(sentence, sizeof(sentence), stdin) == NULL) {
        return 1;
    }

    while (sentence[read_index] != '\0' &&
           sentence[read_index] != '\n') {

        char current = sentence[read_index];

        if (current == ' ' || current == '\t') {
            if (inside_word) {
                sentence[write_index] = ' ';
                write_index++;
                inside_word = 0;
            }
        } else {
            if (!inside_word) {
                word_count++;
                inside_word = 1;
            }

            sentence[write_index] = current;
            write_index++;
        }

        read_index++;
    }

    if (write_index > 0 &&
        sentence[write_index - 1] == ' ') {
        write_index--;
    }

    sentence[write_index] = '\0';

    printf("Normalized sentence: %s\n", sentence);
    printf("Number of words: %d\n", word_count);

    return 0;
}

```

## **Section B — Strings Using Standard Library Functions**

In this section, include `<string.h>` to use the string manipulation functions provided by the C standard library.

### **Problem 8. Determine String Length Using&#x20;**`strlen()`

**Problem statement:** Write a C program that reads a complete sentence and determines its length using the standard library function `strlen()`.

The program must remove the trailing newline character, if present, using `strcspn()` before calculating the length. Display the string and its length. Compare the conceptual working of `strlen()` with the manual traversal implemented in Problem 1.

```

#include <stdio.h>
#include <string.h>

int main(void) {
    char text[150];
    size_t length;

    printf("Enter a string: ");

    if (fgets(text, sizeof(text), stdin) == NULL) {
        return 1;
    }

    text[strcspn(text, "\n")] = '\0';

    length = strlen(text);

    printf("Entered string: %s\n", text);
    printf("Length: %zu\n", length);

    return 0;
}

```

### **Problem 9. Construct a Full Name Using&#x20;**`strcpy()`**&#x20;and&#x20;**`strcat()`

**Problem statement:** Write a C program that accepts the first name and last name of a person in separate character arrays.

Using `strcpy()`, copy the first name into a third character array. Subsequently, use `strcat()` to append a single space followed by the last name.

Display the resulting full name and determine its length using `strlen()`. Ensure that the destination character array is sufficiently large to store the resulting string and its null terminator.

```

#include <stdio.h>
#include <string.h>

int main(void) {
    char first_name[80];
    char last_name[80];
    char full_name[161];

    printf("Enter first name: ");

    if (fgets(first_name, sizeof(first_name), stdin) == NULL) {
        return 1;
    }

    printf("Enter last name: ");

    if (fgets(last_name, sizeof(last_name), stdin) == NULL) {
        return 1;
    }

    first_name[strcspn(first_name, "\n")] = '\0';
    last_name[strcspn(last_name, "\n")] = '\0';

    if (first_name[0] == '\0' || last_name[0] == '\0') {
        printf("Both names are required.\n");
        return 1;
    }

    strcpy(full_name, first_name);
    strcat(full_name, " ");
    strcat(full_name, last_name);

    printf("Full name: %s\n", full_name);
    printf("Length: %zu\n", strlen(full_name));

    return 0;
}

```

### **Problem 10. Sort an Array of Names Alphabetically Using&#x20;**`strcmp()`

**Problem statement:** Write a C program that reads five names and stores them in a two-dimensional character array.

Sort the names in ascending lexicographical order using the Bubble Sort algorithm. The program must use `strcmp()` to compare two strings and `strcpy()` to interchange their contents whenever they are found in the incorrect order.

Display the names in their original order and again after sorting. The sorting must be case-sensitive, following the character comparison rules of `strcmp()`.

**Sample input:**

```
Rohan
Amit
Zoya
Neha
Karan
```

**Expected sorted output:**

```
Amit
Karan
Neha
Rohan
Zoya
```

```

#include <stdio.h>
#include <string.h>

int main(void) {
    char names[5][60];
    char temp[60];

    printf("Enter five names:\n");

    for (int index = 0; index < 5; index++) {
        if (fgets(names[index], sizeof(names[index]), stdin) == NULL) {
            return 1;
        }

        names[index][strcspn(names[index], "\n")] = '\0';
    }

    printf("\nOriginal names:\n");

    for (int index = 0; index < 5; index++) {
        printf("%s\n", names[index]);
    }

    for (int pass = 0; pass < 4; pass++) {
        for (int index = 0; index < 4 - pass; index++) {
            if (strcmp(names[index], names[index + 1]) > 0) {
                strcpy(temp, names[index]);
                strcpy(names[index], names[index + 1]);
                strcpy(names[index + 1], temp);
            }
        }
    }

    printf("\nNames in sorted order:\n");

    for (int index = 0; index < 5; index++) {
        printf("%s\n", names[index]);
    }

    return 0;
}

```

### **Problem 11. Search for Characters and Substrings Using Library Functions**

**Problem statement:** Write a C program that reads a sentence, a character to search for, and a substring.

Using `strchr()`, locate the first occurrence of the specified character. Using `strrchr()`, determine its last occurrence. Subsequently, use `strstr()` to identify all occurrences of the specified substring in the sentence, including overlapping matches.

Display the positions of the matches using zero-based indexing. If a character or substring does not exist, print an appropriate message.

**Example:** For the sentence `banana`, searching for character `a` should return first index `1` and last index `5`. Searching for substring `ana` should report indices `1` and `3`.

```

#include <stdio.h>
#include <string.h>

int main(void) {
    char sentence[200];
    char character_input[20];
    char substring[80];

    char target;
    char *first_position;
    char *last_position;
    char *match;

    int occurrences = 0;

    printf("Enter a sentence: ");

    if (fgets(sentence, sizeof(sentence), stdin) == NULL) {
        return 1;
    }

    printf("Enter character to search: ");

    if (fgets(character_input, sizeof(character_input), stdin) == NULL) {
        return 1;
    }

    printf("Enter substring to search: ");

    if (fgets(substring, sizeof(substring), stdin) == NULL) {
        return 1;
    }

    sentence[strcspn(sentence, "\n")] = '\0';
    character_input[strcspn(character_input, "\n")] = '\0';
    substring[strcspn(substring, "\n")] = '\0';

    if (character_input[0] == '\0' || substring[0] == '\0') {
        printf("Search character and substring cannot be empty.\n");
        return 1;
    }

    target = character_input[0];

    first_position = strchr(sentence, target);
    last_position = strrchr(sentence, target);

    if (first_position != NULL) {
        printf("First character occurrence: %td\n",
               first_position - sentence);

        printf("Last character occurrence: %td\n",
               last_position - sentence);
    } else {
        printf("Character not found.\n");
    }

    printf("Substring matching positions: ");

    match = strstr(sentence, substring);

    while (match != NULL) {
        printf("%td ", match - sentence);
        occurrences++;

        match = strstr(match + 1, substring);
    }

    if (occurrences == 0) {
        printf("None");
    }

    printf("\nTotal substring occurrences: %d\n", occurrences);

    return 0;
}

```

### **Problem 12. Split a Sentence into Words Using&#x20;**`strtok()`

**Problem statement:** Write a C program that accepts a complete sentence and separates it into individual words using the standard library function `strtok()`.

Treat spaces, tabs, commas, periods, semicolons, colons, exclamation marks, and question marks as word delimiters. Consecutive delimiters must not produce empty words.

Display each extracted word on a separate line along with its sequential position, and calculate the total number of words.

The program should demonstrate that `strtok()` modifies the original character array by replacing selected delimiters with null terminators.

**Sample input:** `Hello, C programming! Strings are useful.`

**Expected output:**

```
Word 1: Hello
Word 2: C
Word 3: programming
Word 4: Strings
Word 5: are
Word 6: useful

Total words: 6
```

```

#include <stdio.h>
#include <string.h>

int main(void) {
    char sentence[200];
    char delimiters[] = " \t,.;:!?";
    char *token;

    int word_count = 0;

    printf("Enter a sentence: ");

    if (fgets(sentence, sizeof(sentence), stdin) == NULL) {
        return 1;
    }

    sentence[strcspn(sentence, "\n")] = '\0';

    token = strtok(sentence, delimiters);

    printf("\nExtracted words:\n");

    while (token != NULL) {
        word_count++;

        printf("Word %d: %s\n", word_count, token);

        token = strtok(NULL, delimiters);
    }

    printf("\nTotal words: %d\n", word_count);

    return 0;
}

```

---

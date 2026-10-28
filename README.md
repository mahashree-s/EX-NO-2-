## EX. NO:2 IMPLEMENTATION OF PLAYFAIR CIPHER

 

## AIM:
 

 

To write a C program to implement the Playfair Substitution technique.

## DESCRIPTION:

The Playfair cipher starts with creating a key table. The key table is a 5×5 grid of letters that will act as the key for encrypting your plaintext. Each of the 25 letters must be unique and one letter of the alphabet is omitted from the table (as there are 25 spots and 26 letters in the alphabet).

To encrypt a message, one would break the message into digrams (groups of 2 letters) such that, for example, "HelloWorld" becomes "HE LL OW OR LD", and map them out on the key table. The two letters of the diagram are considered as the opposite corners of a rectangle in the key table. Note the relative position of the corners of this rectangle. Then apply the following 4 rules, in order, to each pair of letters in the plaintext:
1.	If both letters are the same (or only one letter is left), add an "X" after the first letter
2.	If the letters appear on the same row of your table, replace them with the letters to their immediate right respectively
3.	If the letters appear on the same column of your table, replace them with the letters immediately below respectively
4.	If the letters are not on the same row or column, replace them with the letters on the same row respectively but at the other pair of corners of the rectangle defined by the original pair.
## EXAMPLE:
![image](https://github.com/Hemamanigandan/EX-NO-2-/assets/149653568/e6858d4f-b122-42ba-acdb-db18ec2e9675)

 

## ALGORITHM:

STEP-1: Read the plain text from the user.

STEP-2: Read the keyword from the user.

STEP-3: Arrange the keyword without duplicates in a 5*5 matrix in the row order and fill the remaining cells with missed out letters in alphabetical order. Note that ‘i’ and ‘j’ takes the same cell.

STEP-4: Group the plain text in pairs and match the corresponding corner letters by forming a rectangular grid.

STEP-5: Display the obtained cipher text.




Program:
```c

#include <stdio.h>
#include <string.h>

#define SIZE 100

void generateKey(char key[], char t[5][5]) {
    int used[26] = {0}, i = 0, j = 0;
    used['j' - 'a'] = 1;

    for (int k = 0; key[k]; k++) {
        char c = key[k];
        if (c >= 'A' && c <= 'Z') c += 32;
        if (c == 'j') c = 'i';

        if (c >= 'a' && c <= 'z' && !used[c - 'a']) {
            t[i][j++] = c;
            used[c - 'a'] = 1;
            if (j == 5) j = 0, i++;
        }
    }

    for (int k = 0; k < 26; k++) {
        if (!used[k]) {
            t[i][j++] = 'a' + k;
            if (j == 5) j = 0, i++;
        }
    }
}

void encrypt(char s[], char t[5][5], int n) {
    int r1, c1, r2, c2;

    for (int k = 0; k < n; k += 2) {
        char a = s[k], b = s[k + 1];
        if (a == 'j') a = 'i';
        if (b == 'j') b = 'i';

        for (int i = 0; i < 5; i++)
            for (int j = 0; j < 5; j++) {
                if (t[i][j] == a) r1 = i, c1 = j;
                if (t[i][j] == b) r2 = i, c2 = j;
            }

        if (r1 == r2) {
            s[k] = t[r1][(c1 + 1) % 5];
            s[k + 1] = t[r2][(c2 + 1) % 5];
        } else if (c1 == c2) {
            s[k] = t[(r1 + 1) % 5][c1];
            s[k + 1] = t[(r2 + 1) % 5][c2];
        } else {
            s[k] = t[r1][c2];
            s[k + 1] = t[r2][c1];
        }
    }
}

int main() {
    char key[SIZE], plain[SIZE], t[5][5], s[SIZE];
    int n = 0;

    printf("Enter keyword: ");
    fgets(key, SIZE, stdin);
    key[strcspn(key, "\n")] = '\0';

    printf("Enter plaintext: ");
    fgets(plain, SIZE, stdin);
    plain[strcspn(plain, "\n")] = '\0';

    int j = 0;
    for (int i = 0; plain[i]; i++) {
        char c = plain[i];
        if (c >= 'A' && c <= 'Z') c += 32;
        if (c >= 'a' && c <= 'z') s[j++] = c;
    }
    s[j] = '\0';

    for (int i = 0; i < j; i++) {
        if (s[i] == 'j') s[i] = 'i';
    }

    for (int i = 0; i < j; i++) {
        s[n++] = s[i];
        if (i + 1 < j && s[i] == s[i + 1]) {
            s[n++] = 'x';
        }
    }

    if (n % 2 != 0) s[n++] = 'x';
    s[n] = '\0';

    generateKey(key, t);
    encrypt(s, t, n);

    printf("Cipher text: %s\n", s);
    return 0;
}

```



Output:

<img width="1121" height="779" alt="image" src="https://github.com/user-attachments/assets/f60879f1-976d-4654-9b23-8e4678f7f8ed" />

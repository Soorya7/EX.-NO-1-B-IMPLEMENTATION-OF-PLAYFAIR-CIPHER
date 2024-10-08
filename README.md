# EX.-NO-1-B-IMPLEMENTATION-OF-PLAYFAIR-CIPHER

## AIM:
  To write a C program to implement the Playfair Substitution technique.
  
## ALGORITHM:

STEP-1: Read the plain text from the user.

STEP-2: Read the keyword from the user.

STEP-3: Arrange the keyword without duplicates in a 5*5 matrix in the row order and fill the remaining cells with missed out letters in alphabetical order. Note that ‘i’ and ‘j’ takes the same cell.

STEP-4: Group the plain text in pairs and match the corresponding corner letters by forming a rectangular grid.

STEP-5: Display the obtained cipher text.

## PROGRAM:

    #include <stdio.h>
    #include <string.h>
    #include <ctype.h>
    void prepareMatrix(char key[], char matrix[5][5]);
    void prepareText(char text[]);
    void findPosition(char matrix[5][5], char letter, int *row, int *col);
    void encryptPair(char matrix[5][5], char a, char b, char *c1, char *c2);
    void encryptText(char text[], char key[], char cipher[]);

    int main() {
    char text[100], key[25], cipher[100];
    printf("Enter the plain text: ");
    gets(text);
    printf("Enter the keyword: ");
    gets(key);
    encryptText(text, key, cipher);
    printf("Cipher text: %s\n", cipher);
    return 0;
    }
       void prepareMatrix(char key[], char matrix[5][5]) {
    int i, j, k, flag[26] = {0}, idx = 0;
    char alphabet = 'A';
    for(i = 0; i < strlen(key); i++) {
        key[i] = toupper(key[i]);
        if (key[i] == 'J') key[i] = 'I'; 
        if (!flag[key[i] - 'A']) {
            flag[key[i] - 'A'] = 1;
            matrix[idx / 5][idx % 5] = key[i];
            idx++;
        }
    }

    for(i = 0; i < 26; i++) {
        if (!flag[i] && (alphabet + i) != 'J') {
            matrix[idx / 5][idx % 5] = alphabet + i;
            idx++;
    }
    }
    }
    void prepareText(char text[]) 
    {
    int i, j = 0;
    char prepared[100];
    
    for(i = 0; i < strlen(text); i++) {
        if (isalpha(text[i])) {
            prepared[j++] = toupper(text[i]);
        }
    }
    prepared[j] = '\0';
    strcpy(text, prepared);
    }
    void findPosition(char matrix[5][5], char letter, int *row, int *col) {
    int i, j;
    for(i = 0; i < 5; i++) {
        for(j = 0; j < 5; j++) {
            if (matrix[i][j] == letter) {
                *row = i;
                *col = j;
                return;
            }
        }
    }
    }
    void encryptPair(char matrix[5][5], char a, char b, char *c1, char *c2) {
    int r1, c1_pos, r2, c2_pos;

    findPosition(matrix, a, &r1, &c1_pos);
    findPosition(matrix, b, &r2, &c2_pos);

    if (r1 == r2) {
        *c1 = matrix[r1][(c1_pos + 1) % 5];
        *c2 = matrix[r2][(c2_pos + 1) % 5];
    } else if (c1_pos == c2_pos) {
        *c1 = matrix[(r1 + 1) % 5][c1_pos];
        *c2 = matrix[(r2 + 1) % 5][c2_pos];
    } else {
        *c1 = matrix[r1][c2_pos];
        *c2 = matrix[r2][c1_pos];
    }
    }

    void encryptText(char text[], char key[], char cipher[]) {
    char matrix[5][5], bigram[2];
    int i, j = 0, k = 0;

    prepareText(text);
    prepareMatrix(key, matrix);

    for(i = 0; i < strlen(text); i += 2) {
        bigram[0] = text[i];
        if (i + 1 < strlen(text)) {
            bigram[1] = (text[i] == text[i + 1]) ? 'X' : text[i + 1];
        } else {
            bigram[1] = 'X';
        }
        encryptPair(matrix, bigram[0], bigram[1], &cipher[j], &cipher[j + 1]);
        j += 2;
    }
    cipher[j] = '\0';
  }


## OUTPUT:
Enter the plain text: good morning

Enter the keyword: wir

Cipher text: CTPCHSBLBKEZ
## RESULT:
  Thus the Playfair cipher substitution technique had been implemented successfully.

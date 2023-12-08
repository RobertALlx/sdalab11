# sdalab11
Balan Robert
Cosolan Carmen
Ciurescu Radu

#include <stdio.h>
#include <stdlib.h>
#include <string.h>

typedef struct Student {
    char nume[50];
    char prenume[50];
    char cnp[14];
    float medie;
    int varsta;
    struct Student* left;
    struct Student* right;
} Student;

// Functie pentru alocarea unui nou student
Student* createStudent(char nume[], char prenume[], char cnp[], float medie, int varsta) {
    Student* newStudent = (Student*)malloc(sizeof(Student));
    strcpy(newStudent->nume, nume);
    strcpy(newStudent->prenume, prenume);
    strcpy(newStudent->cnp, cnp);
    newStudent->medie = medie;
    newStudent->varsta = varsta;
    newStudent->left = newStudent->right = NULL;
    return newStudent;
}

// Functie pentru a calcula rata de angajare
int calculeazaRataAngajare(char cnp[]) {
    int sumaCifre = 0;
    for (int i = 0; i < 13; ++i) {
        sumaCifre += cnp[i] - '0';
    }
    return sumaCifre;
}


// Functie pentru adaugarea unui student în arbore
Student* adaugaStudent(Student* root, char nume[], char prenume[], char cnp[], float medie, int varsta) {
    if (root == NULL) {
        return createStudent(nume, prenume, cnp, medie, varsta);
    }

    int rataAngajareNou = calculeazaRataAngajare(cnp);
    int rataAngajareCurent = calculeazaRataAngajare(root->cnp);

    if (rataAngajareNou < rataAngajareCurent) {
        root->left = adaugaStudent(root->left, nume, prenume, cnp, medie, varsta);
    }
    else {
        root->right = adaugaStudent(root->right, nume, prenume, cnp, medie, varsta);
    }

    return root;
}

// Functie pentru a afisa studentii în ordine lexicografica
void afiseazaInOrdine(Student* root) {
    if (root != NULL) {
        afiseazaInOrdine(root->left);
        printf("Nume: %s, Prenume: %s, CNP: %s, Medie: %.2f, Varsta: %d\n",
            root->nume, root->prenume, root->cnp, root->medie, root->varsta);
        afiseazaInOrdine(root->right);
    }
}

// Functie pentru eliberarea memoriei alocate pentru arbore
void elibereazaMemorie(Student* root) {
    if (root != NULL) {
        elibereazaMemorie(root->left);
        elibereazaMemorie(root->right);
        free(root);
    }
}

int main() {
    Student* radacina = NULL;

    // Exemplu de utilizare
    radacina = adaugaStudent(radacina, "Ionut", "Popescu", "1234567890123", 8.5, 20);
    radacina = adaugaStudent(radacina, "Vasile", "Ionescu", "9876543210123", 9.0, 22);
    radacina = adaugaStudent(radacina, "Marcel", "Avram", "5555555555555", 7.8, 21);

    printf("Studenti in ordine dupa rata de angajare:\n");
    afiseazaInOrdine(radacina);

    elibereazaMemorie(radacina);

    return 0;
}

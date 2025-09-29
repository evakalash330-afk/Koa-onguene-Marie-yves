README.md# Devoir INF 231 – Structures de données II

## Auteur
- **Nom :** Koa Onguene Marie Yves  
- **Matricule :** 24F2646  

---

## Exercices

1. Lire un élément et supprimer toutes ses occurrences dans une **liste simplement chaînée**.  
2. Insertion d’un élément dans une **liste simplement chaînée triée**.  
3. Insertion d’un élément dans une **liste doublement chaînée triée**.  
4. Insertion en tête et en queue dans une **liste simplement chaînée circulaire**.  
5. Insertion en tête et en queue dans une **liste doublement chaînée circulaire**.  

---

## Compilation et Exécution

Pour compiler chaque fichier :

```bash
gcc ex1_supp_occurrence.c -o ex1
gcc ex2_insertion_scl_trie.c -o ex2
gcc ex3_insertion_dcl_trie.c -o ex3
gcc ex4_insertion_scl_circulaire.c -o ex4
gcc ex5_insertion_dcl_circulaire.c -o ex5
```

Pour exécuter :

```bash
./ex1
./ex2
./ex3
./ex4
./ex5
```
ex1_supp_occurrence.c#include <stdio.h>
#include <stdlib.h>

typedef struct Node {
    int data;
    struct Node* next;
} Node;

Node* insererEnFin(Node* head, int val) {
    Node* newNode = (Node*)malloc(sizeof(Node));
    newNode->data = val;
    newNode->next = NULL;

    if (head == NULL) return newNode;

    Node* temp = head;
    while (temp->next != NULL) temp = temp->next;
    temp->next = newNode;
    return head;
}

Node* supprimerOccurrences(Node* head, int val) {
    Node* temp = head;
    Node* prev = NULL;

    while (temp != NULL) {
        if (temp->data == val) {
            Node* toDelete = temp;
            if (prev == NULL) head = temp->next;
            else prev->next = temp->next;
            temp = temp->next;
            free(toDelete);
        } else {
            prev = temp;
            temp = temp->next;
        }
    }
    return head;
}

void afficher(Node* head) {
    Node* temp = head;
    while (temp != NULL) {
        printf("%d -> ", temp->data);
        temp = temp->next;
    }
    printf("NULL\n");
}

int main() {
    Node* liste = NULL;
    int n, val, x;

    printf("Combien d'elements ? ");
    scanf("%d", &n);

    for (int i = 0; i < n; i++) {
        printf("Entrer element %d : ", i+1);
        scanf("%d", &val);
        liste = insererEnFin(liste, val);
    }

    printf("Liste initiale : ");
    afficher(liste);

    printf("Entrer la valeur a supprimer : ");
    scanf("%d", &x);

    liste = supprimerOccurrences(liste, x);

    printf("Liste apres suppression : ");
    afficher(liste);

    return 0;
}
ex2_insertion_scl_trie.c#include <stdio.h>
#include <stdlib.h>

typedef struct Node {
    int data;
    struct Node* next;
} Node;

Node* insererTrie(Node* head, int val) {
    Node* newNode = (Node*)malloc(sizeof(Node));
    newNode->data = val;
    newNode->next = NULL;

    if (head == NULL || val < head->data) {
        newNode->next = head;
        return newNode;
    }

    Node* temp = head;
    while (temp->next != NULL && temp->next->data < val) {
        temp = temp->next;
    }
    newNode->next = temp->next;
    temp->next = newNode;

    return head;
}

void afficher(Node* head) {
    Node* temp = head;
    while (temp != NULL) {
        printf("%d -> ", temp->data);
        temp = temp->next;
    }
    printf("NULL\n");
}

int main() {
    Node* liste = NULL;
    int n, val;

    printf("Combien d'elements ? ");
    scanf("%d", &n);

    for (int i = 0; i < n; i++) {
        printf("Entrer element %d : ", i+1);
        scanf("%d", &val);
        liste = insererTrie(liste, val);
    }

    printf("Liste triee : ");
    afficher(liste);

    return 0;
}
ex3_insertion_dcl_trie.c#include <stdio.h>
#include <stdlib.h>

typedef struct Node {
    int data;
    struct Node* prev;
    struct Node* next;
} Node;

Node* insererTrie(Node* head, int val) {
    Node* newNode = (Node*)malloc(sizeof(Node));
    newNode->data = val;
    newNode->prev = NULL;
    newNode->next = NULL;

    if (head == NULL || val < head->data) {
        newNode->next = head;
        if (head != NULL) head->prev = newNode;
        return newNode;
    }

    Node* temp = head;
    while (temp->next != NULL && temp->next->data < val) {
        temp = temp->next;
    }

    newNode->next = temp->next;
    if (temp->next != NULL) temp->next->prev = newNode;
    temp->next = newNode;
    newNode->prev = temp;

    return head;
}

void afficher(Node* head) {
    Node* temp = head;
    while (temp != NULL) {
        printf("%d <-> ", temp->data);
        temp = temp->next;
    }
    printf("NULL\n");
}

int main() {
    Node* liste = NULL;
    int n, val;

    printf("Combien d'elements ? ");
    scanf("%d", &n);

    for (int i = 0; i < n; i++) {
        printf("Entrer element %d : ", i+1);
        scanf("%d", &val);
        liste = insererTrie(liste, val);
    }

    printf("Liste doublement chainee triee : ");
    afficher(liste);

    return 0;
}
ex4_insertion_scl_circulaire.c#include <stdio.h>
#include <stdlib.h>

typedef struct Node {
    int data;
    struct Node* next;
} Node;

Node* insererEnTete(Node* last, int val) {
    Node* newNode = (Node*)malloc(sizeof(Node));
    newNode->data = val;

    if (last == NULL) {
        newNode->next = newNode;
        return newNode;
    }

    newNode->next = last->next;
    last->next = newNode;
    return last;
}

Node* insererEnQueue(Node* last, int val) {
    Node* newNode = (Node*)malloc(sizeof(Node));
    newNode->data = val;

    if (last == NULL) {
        newNode->next = newNode;
        return newNode;
    }

    newNode->next = last->next;
    last->next = newNode;
    return newNode; 
}

void afficher(Node* last) {
    if (last == NULL) {
        printf("Liste vide\n");
        return;
    }
    Node* temp = last->next;
    do {
        printf("%d -> ", temp->data);
        temp = temp->next;
    } while (temp != last->next);
    printf("(cercle)\n");
}

int main() {
    Node* last = NULL;

    last = insererEnTete(last, 10);
    last = insererEnQueue(last, 20);
    last = insererEnTete(last, 5);
    last = insererEnQueue(last, 30);

    printf("Liste simplement chainee circulaire : ");
    afficher(last);

    return 0;
}
ex5_insertion_dcl_circulaire.c#include <stdio.h>
#include <stdlib.h>

typedef struct Node {
    int data;
    struct Node* prev;
    struct Node* next;
} Node;

Node* insererEnTete(Node* last, int val) {
    Node* newNode = (Node*)malloc(sizeof(Node));
    newNode->data = val;

    if (last == NULL) {
        newNode->next = newNode;
        newNode->prev = newNode;
        return newNode;
    }

    Node* first = last->next;
    newNode->next = first;
    newNode->prev = last;
    first->prev = newNode;
    last->next = newNode;

    return last;
}

Node* insererEnQueue(Node* last, int val) {
    Node* newNode = (Node*)malloc(sizeof(Node));
    newNode->data = val;

    if (last == NULL) {
        newNode->next = newNode;
        newNode->prev = newNode;
        return newNode;
    }

    Node* first = last->next;
    newNode->next = first;
    newNode->prev = last;
    first->prev = newNode;
    last->next = newNode;

    return newNode; 
}

void afficher(Node* last) {
    if (last == NULL) {
        printf("Liste vide\n");
        return;
    }
    Node* temp = last->next;
    do {
        printf("%d <-> ", temp->data);
        temp = temp->next;
    } while (temp != last->next);
    printf("(cercle)\n");
}

int main() {
    Node* last = NULL;

    last = insererEnTete(last, 10);
    last = insererEnQueue(last, 20);
    last = insererEnTete(last, 5);
    last = insererEnQueue(last, 30);

    printf("Liste doublement chainee circulaire : ");
    afficher(last);

    return 0;
}

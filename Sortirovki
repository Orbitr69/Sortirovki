#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <time.h>

// Копирование массива
void copyArray(int* dest, int* src, int n) {
    for (int i = 0; i < n; i++)
        dest[i] = src[i];
}

// Проверка, отсортирован ли массив
int isSorted(int a[], int n) {
    for (int i = 1; i < n; i++)
        if (a[i] < a[i-1]) return 0;
    return 1;
}

void printArray(int a[], int n) {
    for (int i = 0; i < n; i++)
        printf("%d ", a[i]);
    printf("\n");
}

// 1. Insertion Sort
void insertionSort(int a[], int n) {
    for (int i = 1; i < n; i++) {
        int k = a[i];
        int j = i - 1;
        while (j >= 0 && a[j] > k) {
            a[j + 1] = a[j];
            j--;
        }
        a[j + 1] = k;
    }
}

// 2. Selection Sort
void selectionSort(int a[], int n) {
    for (int i = 0; i < n - 1; i++) {
        int m = i;
        for (int j = i + 1; j < n; j++) {
            if (a[j] < a[m])
                m = j;
        }
        if (m != i) {
            int t = a[i];
            a[i] = a[m];
            a[m] = t;
        }
    }
}

// 3. Bubble Sort
void bubbleSort(int a[], int n) {
    for (int i = 0; i < n - 1; i++) {
        int f = 0;
        for (int j = 0; j < n - i - 1; j++) {
            if (a[j] > a[j + 1]) {
                int t = a[j];
                a[j] = a[j + 1];
                a[j + 1] = t;
                f = 1;
            }
        }
        if (!f) break;
    }
}

// 4. Gnome Sort
void gnomeSort(int a[], int n) {
    int i = 0;
    while (i < n) {
        if (i == 0 || a[i - 1] <= a[i])
            i++;
        else {
            int t = a[i];
            a[i] = a[i - 1];
            a[i - 1] = t;
            i--;
        }
    }
}

// 5. Shell Sort
void shellSort(int a[], int n) {
    for (int g = n / 2; g > 0; g /= 2) {
        for (int i = g; i < n; i++) {
            int t = a[i];
            int j;
            for (j = i; j >= g && a[j - g] > t; j -= g)
                a[j] = a[j - g];
            a[j] = t;
        }
    }
}

// 6. qsort
int cmp(const void* a, const void* b) {
    return (*(int*)a - *(int*)b);
}

// Функция для замера времени
double testSort(void (*sortFunc)(int*, int), int a[], int n, char* name) {
    int* b = malloc(n * sizeof(int));
    copyArray(b, a, n);
    
    clock_t start = clock();
    sortFunc(b, n);
    clock_t end = clock();
    
    double t = (double)(end - start) / CLOCKS_PER_SEC;
    
    if (isSorted(b, n))
        printf("%-12s: %.6f сек\n", name, t);
    else
        printf("%-12s: ОШИБКА - не отсортировано!\n", name);
    
    free(b);
    return t;
}

int main() {
    // Размеры массивов для тестов
    int sizes[] = {1000, 5000, 10000, 20000};
    int numSizes = sizeof(sizes) / sizeof(sizes[0]);
    
    printf("==================================================\n");
    printf("Сравнение скорости сортировок\n");
    printf("==================================================\n\n");
    
    for (int s = 0; s < numSizes; s++) {
        int n = sizes[s];
        printf("Размер массива: %d элементов\n", n);
        printf("--------------------------------------------------\n");
        
        // Создаём случайный массив
        int* original = malloc(n * sizeof(int));
        srand(42); // фиксированный seed для повторяемости
        for (int i = 0; i < n; i++)
            original[i] = rand() % 10000;
        
        // Замеряем каждую сортировку
        testSort(insertionSort, original, n, "Insertion");
        testSort(selectionSort, original, n, "Selection");
        testSort(bubbleSort, original, n, "Bubble");
        testSort(gnomeSort, original, n, "Gnome");
        testSort(shellSort, original, n, "Shell");
        testSort((void(*)(int*,int))qsort, original, n, "qsort");
        
        free(original);
        printf("\n");
    }
    
    // Дополнительный тест: почти отсортированный массив
    printf("\n==================================================\n");
    printf("Тест на почти отсортированном массиве (n=20000)\n");
    printf("==================================================\n\n");
    
    int n = 20000;
    int* arr = malloc(n * sizeof(int));
    for (int i = 0; i < n; i++)
        arr[i] = i;
    // Меняем местами 100 случайных пар
    srand(42);
    for (int k = 0; k < 100; k++) {
        int i = rand() % n;
        int j = rand() % n;
        int t = arr[i];
        arr[i] = arr[j];
        arr[j] = t;
    }
    
    testSort(insertionSort, arr, n, "Insertion");
    testSort(selectionSort, arr, n, "Selection");
    testSort(bubbleSort, arr, n, "Bubble");
    testSort(gnomeSort, arr, n, "Gnome");
    testSort(shellSort, arr, n, "Shell");
    testSort((void(*)(int*,int))qsort, arr, n, "qsort");
    
    free(arr);
    
    printf("\n==================================================\n");
    printf("ВЫВОДЫ:\n");
    printf("- qsort и Shell Sort — самые быстрые на больших данных\n");
    printf("- Insertion Sort очень быстр на почти отсортированных\n");
    printf("- Bubble, Selection, Gnome — медленные на больших n\n");
    printf("- Сложность O(n^2) против O(n log n) даёт о себе знать\n");
    printf("==================================================\n");
    
    return 0;
}

# 📚 Métodos de Ordenação em Java (Sorting Algorithms)

Este guia serve como uma referência rápida para os principais algoritmos de ordenação implementados em Java. Ele inclui a lógica básica, a complexidade de tempo (Big O) e a implementação direta de cada um.

## 1. Bubble Sort (Ordenação por Bolha)

Compara pares de elementos adjacentes e os troca de posição se estiverem na ordem errada. A cada iteração, o maior elemento "flutua" para o final do array.

* **Melhor Caso:** $O(n)$ (array já ordenado)
* **Pior/Médio Caso:** $O(n^2)$

```java
public static void bubbleSort(int[] arr) {
    int n = arr.length;
    for (int i = 0; i < n - 1; i++) {
        boolean trocou = false;
        for (int j = 0; j < n - i - 1; j++) {
            if (arr[j] > arr[j + 1]) {
                int temp = arr[j];
                arr[j] = arr[j + 1];
                arr[j + 1] = temp;
                trocou = true;
            }
        }
        // Otimização: se não houve troca, o array já está ordenado
        if (!trocou) break; 
    }
}

```

## 2. Selection Sort (Ordenação por Seleção)

Divide o array em duas partes: a ordenada e a não ordenada. A cada iteração, busca o menor elemento da parte não ordenada e o move para o final da parte ordenada.

* **Complexidade (Todos os casos):** $O(n^2)$

```java
public static void selectionSort(int[] arr) {
    int n = arr.length;
    for (int i = 0; i < n - 1; i++) {
        int indiceMenor = i;
        for (int j = i + 1; j < n; j++) {
            if (arr[j] < arr[indiceMenor]) {
                indiceMenor = j;
            }
        }
        int temp = arr[indiceMenor];
        arr[indiceMenor] = arr[i];
        arr[i] = temp;
    }
}

```

## 3. Insertion Sort (Ordenação por Inserção)

Constrói o array ordenado um elemento por vez, comparando o elemento atual com os anteriores e inserindo-o na posição correta (semelhante a organizar cartas de baralho na mão).

* **Melhor Caso:** $O(n)$
* **Pior/Médio Caso:** $O(n^2)$

```java
public static void insertionSort(int[] arr) {
    int n = arr.length;
    for (int i = 1; i < n; i++) {
        int chave = arr[i];
        int j = i - 1;
        while (j >= 0 && arr[j] > chave) {
            arr[j + 1] = arr[j];
            j = j - 1;
        }
        arr[j + 1] = chave;
    }
}

```

## 4. Merge Sort (Ordenação por Mistura)

Usa a abordagem "Dividir para Conquistar". Divide o array na metade recursivamente até que cada sub-array tenha apenas um elemento, e então os mescla de volta em ordem.

* **Complexidade (Todos os casos):** $O(n \log n)$
* **Complexidade de Espaço:** $O(n)$ (requer array auxiliar)

```java
public static void mergeSort(int[] arr, int inicio, int fim) {
    if (inicio < fim) {
        int meio = (inicio + fim) / 2;
        mergeSort(arr, inicio, meio);
        mergeSort(arr, meio + 1, fim);
        merge(arr, inicio, meio, fim);
    }
}

private static void merge(int[] arr, int inicio, int meio, int fim) {
    int n1 = meio - inicio + 1;
    int n2 = fim - meio;
    int[] esq = new int[n1];
    int[] dir = new int[n2];

    System.arraycopy(arr, inicio, esq, 0, n1);
    for (int j = 0; j < n2; j++) dir[j] = arr[meio + 1 + j];

    int i = 0, j = 0, k = inicio;
    while (i < n1 && j < n2) {
        if (esq[i] <= dir[j]) {
            arr[k++] = esq[i++];
        } else {
            arr[k++] = dir[j++];
        }
    }
    while (i < n1) arr[k++] = esq[i++];
    while (j < n2) arr[k++] = dir[j++];
}

```

## 5. Quick Sort (Ordenação Rápida)

Também usa "Dividir para Conquistar". Escolhe um elemento como "pivô" e particiona o array ao redor dele, colocando os menores à esquerda e os maiores à direita, ordenando as partições recursivamente.

* **Melhor/Médio Caso:** $O(n \log n)$
* **Pior Caso:** $O(n^2)$ (quando o array já está ordenado e o pivô é mal escolhido)

```java
public static void quickSort(int[] arr, int inicio, int fim) {
    if (inicio < fim) {
        int indicePivo = particionar(arr, inicio, fim);
        quickSort(arr, inicio, indicePivo - 1);
        quickSort(arr, indicePivo + 1, fim);
    }
}

private static int particionar(int[] arr, int inicio, int fim) {
    int pivo = arr[fim];
    int i = (inicio - 1);
    for (int j = inicio; j < fim; j++) {
        if (arr[j] <= pivo) {
            i++;
            int temp = arr[i];
            arr[i] = arr[j];
            arr[j] = temp;
        }
    }
    int temp = arr[i + 1];
    arr[i + 1] = arr[fim];
    arr[fim] = temp;
    return i + 1;
}

```

---

## 🚀 Como o Java ordena internamente?

Em aplicações reais, raramente implementamos esses métodos do zero. O Java já fornece métodos altamente otimizados:

* `Arrays.sort(arr)` **para tipos primitivos:** Utiliza o **Dual-Pivot Quicksort** (oferece $O(n \log n)$ na maioria dos cenários e evita o pior caso $O(n^2)$).
* `Collections.sort(list)` **para Objetos:** Utiliza o **Timsort** (um híbrido derivado do Merge Sort e Insertion Sort), que é estável e excelente para dados do mundo real que já possuem alguma ordenação parcial.

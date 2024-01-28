package oy.interact.tira.student;

import java.util.Comparator;

public class Algorithms {

   private Algorithms() {
      // nada
   }

   ///////////////////////////////////////////
   // Insertion Sort for the whole array
   ///////////////////////////////////////////

   public static <T extends Comparable<T>> void insertionSort(T[] array) {
      insertionSort(array, 0, array.length);
   }

   ///////////////////////////////////////////
   // Insertion Sort for a slice of the array
   ///////////////////////////////////////////

   public static <T extends Comparable<T>> void insertionSort(T[] array, int fromIndex, int toIndex) {
      int n = toIndex;
      boolean swapped;

      do {
         swapped = false;
         for (int i = fromIndex + 1; i < n; i++) {
            if (array[i - 1].compareTo(array[i]) > 0) {
               // Swap A[i-1] and A[i]
               T temp = array[i - 1];
               array[i - 1] = array[i];
               array[i] = temp;
               swapped = true;
            }
         }
         n--;
      } while (swapped);

   }

   //////////////////////////////////////////////////////////
   // Insertion Sort for the whole array using a Comparator
   //////////////////////////////////////////////////////////

   public static <T> void insertionSort(T[] array, Comparator<T> comparator) {
      insertionSort(array, 0, array.length, comparator);
   }

   ////////////////////////////////////////////////////////////
   // Insertion Sort for slice of the array using a Comparator
   ////////////////////////////////////////////////////////////

   public static <T> void insertionSort(T[] array, int fromIndex, int toIndex, Comparator<T> comparator) {
      int m = toIndex;

      for (; fromIndex < toIndex - 1; fromIndex++) {
         for (toIndex = fromIndex + 1; toIndex < m; toIndex++) {
            if (comparator.compare(array[fromIndex], array[toIndex]) > 0) {
               swap(array, fromIndex, toIndex);
            }
         }
      }
   }

   private static <T> void swap(T[] array, int m, int n) {
      final T temp = array[m];
      array[m] = array[n];
      array[n] = temp;
   }

   ///////////////////////////////////////////
   // Reversing an array
   ///////////////////////////////////////////

   public static <T> void reverse(T[] array) {
      reverse(array, 0, array.length);
   }

   ///////////////////////////////////////////
   // Reversing a slice of an array
   ///////////////////////////////////////////

   public static <T> void reverse(T[] array, int fromIndex, int toIndex) {
      int n = toIndex - 1;
      int m = fromIndex;

      do {
         T temp = array[m];
         array[m] = array[n];
         array[n] = temp;
         m++;
         n--;
      } while (n > m);
   }

   ///////////////////////////////////////////
   // Binary search bw indices
   ///////////////////////////////////////////

   public static <T extends Comparable<T>> int binarySearch(T aValue, T[] fromArray, int fromIndex, int toIndex) {

      toIndex--;

      while (fromIndex <= toIndex) {
         // int middle = fromIndex + (toIndex -1) / 2;
         int middle = toIndex - (toIndex - fromIndex) / 2;

         if (fromArray[middle].compareTo(aValue) == 0) {
            return middle;
         }
         if (fromArray[middle].compareTo(aValue) < 0) {
            fromIndex = middle + 1;

         } else {
            toIndex = middle - 1;
         }
      }

      return -1;

   }

   ///////////////////////////////////////////
   // Binary search using a Comparator
   ///////////////////////////////////////////

   public static <T> int binarySearch(T aValue, T[] fromArray, int fromIndex, int toIndex, Comparator<T> comparator) {

      toIndex--;

      while (fromIndex <= toIndex) {
         // int middle = fromIndex + (toIndex -1) / 2;
         int middle = toIndex - (toIndex - fromIndex) / 2;
         if (comparator.compare(fromArray[middle], aValue) == 0) {
            return middle;
         }
         // if (fromArray[middle].compare(aValue) == -1){
         if (comparator.compare(fromArray[middle], aValue) < 0) {
            fromIndex = middle + 1;

         } else {
            toIndex = middle - 1;
         }
      }
      return -1;
   }

   public static <E extends Comparable<E>> void fastSort(E[] array) {
      heapSort(array, 0, array.length - 1, Comparator.naturalOrder());
      // quickSort(array,0, array.length -1, Comparator.naturalOrder());
      // mergeSort(array, Comparator.naturalOrder());

      // heapSort(array);
   }

   public static <E> void fastSort(E[] array, Comparator<E> comparator) {
      heapSort(array, 0, array.length - 1, comparator);
      // quickSort(array,0, array.length -1, comparator);
      // mergeSort(array,0, array.length, comparator);

   }

   public static <E> void fastSort(E[] array, int fromIndex, int toIndex, Comparator<E> comparator) {
      heapSort(array, fromIndex, toIndex, comparator);
      // quickSort(array, fromIndex, toIndex - 1, comparator);
      // mergeSort(array, fromIndex, toIndex, comparator);
   }

   public static <E> void mergeSort(E[] array, int fromIndex, int toIndex, Comparator<E> comparator) {

      if (fromIndex > 0 || toIndex < array.length) {
         E[] temp = (E[]) new Object[toIndex - fromIndex];
         int k = 0;
         for (int i = fromIndex; i < toIndex; i++, k++) {
            temp[k] = array[i];
         }
         mergeSort(temp, comparator);
         k = 0;
         for (int i = fromIndex; i < toIndex; i++, k++) {
            array[i] = temp[k];
         }
      } else {
         mergeSort(array, comparator);
      }

   }

   public static <E> void mergeSort(E[] array, Comparator<E> comparator) {

      if (array.length == 1) {
         return;
      }
      int middle = (array.length) / 2;
      // int left = middle-fromIndex + 1;
      // int right = toIndex- middle - 1;
      E[] leftArray = (E[]) new Object[middle];
      E[] rightArray = (E[]) new Object[array.length - middle];
      for (int i = 0; i < middle; i++) {
         leftArray[i] = array[i];
      }
      int k = 0;
      for (int i = middle; i < array.length; i++) {
         rightArray[k++] = array[i];
      }

      mergeSort(leftArray, comparator);
      mergeSort(rightArray, comparator);
      mergeFrom(leftArray, rightArray, array, comparator);

   }

   public static <E> void mergeFrom(E[] leftArray, E[] rightArray, E[] array, Comparator<E> comparator) {

      int i = 0, j = 0, k = 0;
      while (i < leftArray.length && j < rightArray.length) {
         if (comparator.compare(leftArray[i], rightArray[j]) <= 0) {
            array[k++] = leftArray[i++];
         } else {
            array[k++] = rightArray[j++];
         }
      }
      while (i < leftArray.length) {
         array[k++] = leftArray[i++];
      }
      while (j < rightArray.length) {
         array[k++] = rightArray[j++];
      }

   }

   // https://www.baeldung.com/java-merge-sort

   public static <E> void quickSort(E[] array, int fromIndex, int toIndex, Comparator<E> comparator) {

      // if(comparator.compare(fromIndex, toIndex) < 0){
      if (fromIndex < toIndex) {
         int partitionIndex = partition(array, fromIndex, toIndex, comparator);
         quickSort(array, fromIndex, partitionIndex - 1, comparator);
         quickSort(array, partitionIndex + 1, toIndex, comparator);
      }

   }

   public static <E> int partition(E[] array, int fromIndex, int toIndex, Comparator<E> comparator) {
      E pivot = array[toIndex];
      int i = fromIndex - 1;
      for (int j = fromIndex; j < toIndex; j++) {
         if (comparator.compare(array[j], pivot) <= 0) {
            // if(array[j] <= pivot){
            i += 1;
            swap(array, j, i);
         }
      }
      swap(array, toIndex, i + 1);
      return i + 1;

   }

   public static <E> void heapSort(E[] array, Comparator<E> comparator) {
      heapSort(array, 0, array.length - 1, comparator);
   }

   public static <E> void heapSort(E[] array, int fromIndex, int toIndex, Comparator<E> comparator) {

      if (fromIndex == 0 && toIndex == array.length - 1) {
         heapify(array, toIndex, comparator);
         while (toIndex > fromIndex) {
            swap(array, 0, toIndex);
            toIndex -= 1;
            siftDown(array, fromIndex, toIndex, comparator);
         }
      } else {
         E[] temp = (E[]) new Object[toIndex - fromIndex];
         int k = 0;
         for (int i = fromIndex; i < toIndex; i++, k++) {
            temp[k] = array[i];
         }
         heapSort(temp, comparator);
         k = 0;
         for (int i = fromIndex; i < toIndex; i++, k++) {
            array[i] = temp[k];
         }
      }
   }

   public static <E> void heapify(E[] array, int end, Comparator<E> comparator) {
      int start = 0;
      start = parent(end);
      while (start >= 0) {
         siftDown(array, start, end, comparator);
         start -= 1;
      }
   }

   public static <E> void siftDown(E[] array, int start, int end, Comparator<E> comparator) {
      int root = start;
      while (leftChild(root) <= end) {
         int child = leftChild(root);
         int swap = root;
         if (comparator.compare(array[swap], array[child]) < 0) {
            swap = child;
         }
         if ((child + 1 <= end) && (comparator.compare(array[swap], array[child + 1]) < 0)) {
            // array[swap] < array[child + 1]){
            swap = child + 1;
         }
         if (swap == root) {
            return;
         } else {
            swap(array, root, swap);
            root = swap;
         }
      }
   }

   public static int parent(int i) {
      double value = ((i - 1) / 2);
      return (int) Math.floor(value);
   }

   public static int leftChild(int i) {
      return 2 * i + 1;
   }

   public static int rightChild(int i) {
      return 2 * i + 2;
   }

}

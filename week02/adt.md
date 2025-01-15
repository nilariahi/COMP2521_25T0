# ADTs

1.  Consider the following Set ADT interface:

    ```c
    // Creates a new empty set
    Set SetNew(void);
    
    // Frees memory allocated to the set
    void SetFree(Set s);
    
    // Inserts an element into the set
    void SetInsert(Set s, int elem);
    
    // Deletes an element from the set
    void SetDelete(Set s, int elem);
    
    // Returns true if the given element is in the set, and false otherwise
    bool SetContains(Set s, int elem);
    
    // Returns the number of elements in the set
    int SetSize(Set s);
    ```
    
    Use the Set ADT to implement the following functions:
    
    1.  This function takes an array of integers and its size, and returns the number of integers that occur an odd number of times.
    
        For example, if given the array `[4, 3, 4, 8, 8, 4]`, the function should return 2, because there are two elements that appear an odd number of times: 3 (occurs 1 time) and 4 (occurs 3 times).
    
        ```c
        int numOddOccurrences(int arr[], int size) {
            Set odds = SetNew();
            for (int i = 0; i < size; i++) {
                if (SetContains(odds, arr[i])) {
                    SetDelete(odds, arr[i]);
                } else {
                    SetInsert(odds, arr[i]);
                }
            }
            int numOdds = SetSize(odds);
            SetFree(odds);
            return numOdds;
        }
        ```
    
    2.  This function takes an array of integers and its size, and returns the number of integers that occur exactly once.
    
        For example, if given the array `[4, 3, 4, 8, 7, 4]`, the function should return 3, because there are three elements that occur exactly once: 3, 8 and 7.
    
        ```c
        int numSingleOccurrences(int arr[], int size) {
            Set once = SetNew();
            Set moreThanOnce = SetNew();
            
            for (int i = 0; i < size; i++) {
                if (SetContains(once, arr[i])) {
                    SetDelete(once, arr[i]);
                    SetInsert(moreThanOnce, arr[i]);
                } else if (!SetContains(moreThanOnce, arr[i])) {
                    SetInsert(once, arr[i]);
                }
            }
            
            int ans = SetSize(once);
            SetFree(moreThanOnce);
            SetFree(once);
            return ans;
        }
        ```
    
1.  A classic problem in computer science is the problem of implementing a queue using two stacks. Consider the following stack ADT interface:

    ```c
    // Creates a new empty stack O(1)
    Stack StackNew(void);
    
    // Pushes an item onto the stack O(1)
    void StackPush(Stack s, int item);
    
    // Pops an item from the stack and returns it O(1)
    // Assumes that the stack is not empty
    int StackPop(Stack s);
    
    // Returns the number of items on the stack O(1)
    int StackSize(Stack s);
    ```

    Use the stack ADT interface to complete the implementation of the queue ADT below:

    ```c
    #include "Stack.h"
    
    struct queue {
        Stack s1;
        Stack s2;
    };
    
    Queue QueueNew(void) {
        Queue q = malloc(sizeof(struct queue));
        q->s1 = StackNew();
        q->s2 = StackNew();
        return q;
    }
    
    // O(1)
    void QueueEnqueue(Queue q, int item) {
        StackPush(q->s1, item);
    }
    
    // O(n)
    int QueueDequeue(Queue q) {
        if (StackSize(q->s2) == 0) {
            while (StackSize(q->s1) > 0) {
                int it = StackPop(q->s1);
                StackPush(q->s2, it);
            } 
        }
    
        return StackPop(q->s2);
    }
    ```
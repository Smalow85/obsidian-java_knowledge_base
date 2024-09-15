A **linked list** is a linear data structure that consists of a series of nodes connected references (in Java, Python and JavaScript). Each node contains data and reference to the next node in the list. Unlike **arrays, linked lists** allow for efficient **insertion** or **removal** of elements from any position in the list, as the nodes are not stored contiguously in memory.
### Linked List Applications

- Implementing stacks and queues using linked lists.
- Using linked lists to handle collisions in hash tables.
- Representing graphs using linked lists.
- Allocating and deallocating memory dynamically.

## Reverse a Linked List

### Iterative Method:

Follow the steps below to solve the problem:

- Initialize three pointers **prev** as NULL, **curr** as **head**, and **next** as NULL.
- Iterate through the linked list. In a loop, do the following:
    - Before changing the **next** of **curr**, store the **next** node 
        - next = curr -> next
    - Now update the **next** pointer of **curr** to the **prev**
        - curr -> next = prev 
    - Update **prev** as **curr** and **curr** as **next** 
        - prev = curr 
        - curr = next

```java
class LinkedList {

    static Node head;

    static class Node {

        int data;
        Node next;

        Node(int d)
        {
            data = d;
            next = null;
        }
    }

    /* Function to reverse the linked list */
    Node reverse(Node node)
    {
        Node prev = null;
        Node current = node;
        Node next = null;
        while (current != null) {
            next = current.next;
            current.next = prev;
            prev = current;
            current = next;
        }
        node = prev;
        return node;
    }

    // prints content of double linked list
    void printList(Node node)
    {
        while (node != null) {
            System.out.print(node.data + " ");
            node = node.next;
        }
    }
}
```

### Recursive method:

The idea is to reach the last node of the linked list using recursion then start reversing the linked list.

Follow the steps below to solve the problem:
- Divide the list in two parts – first node and rest of the linked list.
- Call reverse for the rest of the linked list.
- Link the rest linked list to first.
- Fix head pointer to NULL

![[Pasted image 20240728231134.png]]

```java
class recursion {
    static Node head; // head of list

    static class Node {
        int data;
        Node next;
        Node(int d)
        {
            data = d;
            next = null;
        }
    }

    static Node reverse(Node head)
    {
        if (head == null || head.next == null)
            return head;

        /* reverse the rest list and put
        the first element at the end */
        Node rest = reverse(head.next);
        head.next.next = head;

        /* tricky step -- see the diagram */
        head.next = null;

        /* fix the head pointer */
        return rest;
    }
}
```

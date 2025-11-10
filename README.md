# 📘 資料結構考題筆記

---
**Original Question:**  
What are the merits of using *iterators* in a data structure? 【10%】

**中文翻譯：**  
在資料結構中使用「迭代器 (iterator)」有什麼優點？

**Answer：**  
迭代器提供了一種統一的方式歷遍資料結構的元素。

---
**Original Question:**  
What are the advantages of using *Last pointer* to a circular chain? 【10%】

**中文翻譯：**  
在「循環鏈結串列」中使用 *Last* 指標有什麼優點？

**Answer：**  
- 能夠在 O(1) 時間內存取表尾節點  
- 插入新節點到表頭或表尾都更有效率  

---

**Original Question:**  
What is a *data structure*? Show the title and the authors of textbook. 【10%】

**中文翻譯：**  
什麼是資料結構？請寫出課本名稱與作者。

**Answer：**  
- **Definition：**  
  資料結構關注在資料的表達以及操縱。 
- **Textbook：**  
  *Data Structures, Algorithms, and Applications in C++*  
  by Sartaj Sahni

---

**Original Question:**  
An algorithm should satisfy what criteria? 【10%】

**中文翻譯：**  
演算法應該滿足哪些條件？

**Answer：**  
1. **輸入**：零或多個外部供應。  
2. **輸出**：至少一個被產生。  
3. **明確性**：每個操作都是清晰且明確的。  
4. **有限性**：如果我們追朔演算法的操作，在所有的情況下，演算法都在有限步驟後結束。  
5. **有效性**：每個操作都必須足夠基本，原則上只使用紙與筆就能執行。

---
```cpp
template <class T> class Chain; // forward declaration

template <class T>
class ChainNode{
    friend class Chain <T>;
    private:
        T data;
        ChainNode <T> *link;
};

template <class T>
class Chain{
    public:
        Chain () {first = 0;}; // constructor initializing first to 0
        // Chain manipulation operations
    
    private:
        ChainNode <T> *first;
}
```
**Original Question:**  
請實作下列 constructors for class ChaniNode 【15%】
```cpp
ChainNode(){}
ChainNode(const T& data)
ChainNode(const T& data, chainNode<T>* link)
```

**Answer：**  
```cpp
ChainNode(){link=nullptr;}
ChainNode(const T& data){data=data;link=nullptr;}
ChainNode(const T& data, chainNode<T>* link){data=data;link=link;}
```

**Original Question:**  
請實作一般的 Linked list:應包含 IndexOf、Delete、Insert 等基本操作 【15%】

**Answer：**  
```cpp
template <class T>
class Chain {
private:
    ChainNode<T>* first; // 指向第一個節點
    int length;          // 方便記錄長度

public:
    Chain() : first(nullptr), length(0) {}
    // 插入：在指定位置插入元素 (0-based)
    void Insert(int index, const T& item) {
        if (index < 0 || index > length) {
            cout << "Index out of range!\n";
            return;
        }

        if (index == 0) { // 插在最前面
            first = new ChainNode<T>(item, first);
        } else {
            ChainNode<T>* prev = first;
            for (int i = 0; i < index - 1; i++)
                prev = prev->link;
            prev->link = new ChainNode<T>(item, prev->link);
        }
        length++;
    }
    // 刪除指定位置節點
    void Delete(int index) {
        if (index < 0 || index >= length) {
            cout << "Index out of range!\n";
            return;
        }

        ChainNode<T>* delNode;
        if (index == 0) {
            delNode = first;
            first = first->link;
        } else {
            ChainNode<T>* prev = first;
            for (int i = 0; i < index - 1; i++)
                prev = prev->link;
            delNode = prev->link;
            prev->link = delNode->link;
        }
        delete delNode;
        length--;
    }
    // 查找指定資料的索引，若找不到則回傳 -1
    int IndexOf(const T& item) const {
        ChainNode<T>* current = first;
        int index = 0;
        while (current != nullptr) {
            if (current->data == item)
                return index;
            current = current->link;
            index++;
        }
        return -1;
    }
};
```
---
```cpp
template <class T>
class Queuе
{// A finite ordered list with zero or more elements.
    public:
        Queue(int queueCapacity = 10);
        //Create an empty queue whose initial capacity is queue Capacity
        bool IsEmpry() const;
        // If number of elements in the queue is 0, return true else return false.
        T& Front() const;
        // Return the element at the front of the queue.
        T& Rear() const;
        //Return the element at the rear of the queue.
        void Push (const T& item);
        / Insert item at the rear of the queue.
        void Pop();
        // Delete the front element of the queue.
};
```
**Original Question:**  
 Implement a circular queue according to the ADT in the following. 【20%】

**中文翻譯：**  
根據下面的抽象資料類型（ADT）實作循環隊列。

**Answer：**  
```cpp
template <class T>
class Queue {
public:
    Queue(int queueCapacity = 10) {
        if (queueCapacity >= 1){
        capacity = queueCapacity;
        queue = new T[capacity];
        front = rear = 0; // 空佇列狀態
        }
    }

    bool IsEmpty() const {
        return front == rear;
    }

    T& Front() const {
        return queue[(front + 1) % capacity];
    }

    T& Rear() const {
        return queue[rear];
    }

    void Push(const T& item) {
        // 判斷是否滿
        if ((rear + 1) % capacity == front) {
            // 需要擴充
            T* newQueue = new T[2 * capacity];
            int start = (front + 1) % capacity;
            if (start < 2) { // 沒有繞圈
                for (int i = 0; i < capacity - 1; i++)
                    newQueue[i] = queue[start + i];
            } else {
                int j = 0;
                for (int i = start; i < capacity; i++)
                    newQueue[j++] = queue[i];
                for (int i = 0; i <= rear; i++)
                    newQueue[j++] = queue[i];
            }
            front = 2 * capacity - 1;
            rear = capacity - 2;
            capacity *= 2;
            delete[] queue;
            queue = newQueue;
        }

        rear = (rear + 1) % capacity;
        queue[rear] = item;
    }

    void Pop() {
        if (IsEmpty())
            throw underflow_error("Queue is empty");
        front = (front + 1) % capacity;
    }

private:
    T* queue;
    int front;
    int rear;
    int capacity;
};
```

**Original Question:**  
How to distinguish between a full queue and an empty queue? 【10%】

**中文翻譯：**  
如何區分滿隊列和空隊列？

**Answer：**  
1. 不要讓queue隊伍排滿 : 當新增元素會導致queue滿時，增加queue容量。
2. 定義一個Bool變數lastOperationIsPush : push時設定為True，pop時設定為False，Queue是空的`iff(front==rear)&&!lastOperationIsPush`，Queue是滿的`iff(front==rear)&&lastOperationIsPush`。  
3. 定義一個Int變數size : push時size++，pop時size--，Queue是空的`size==0`，Queue是滿的`size!=0`
---

**Original Question:**  
請使用 Circular List with Header node 來實作 Linked Stack and Queue 【20%】

**Answer：**  
```cpp
template <class T>
class Stack {
private:
    Node<T>* top; // header node，top->link 指向第一個元素

public:
    Stack(int stackCapacity = 10) {
        top = new Node<T>(); // header node
        top->link = top;     // 指向自己，形成環狀結構
    }

    bool IsEmpty() const {
        return top->link == top;
    }

    T& Top() const {
        if (IsEmpty()) throw runtime_error("Stack is empty");
        return top->link->data;
    }

    void Push(const T& item) {
        Node<T>* newNode = new Node<T>(item);
        newNode->link = top->link;
        top->link = newNode;
    }

    void Pop() {
        if (IsEmpty()) throw runtime_error("Stack is empty");
        Node<T>* temp = top->link;
        top->link = temp->link;
        delete temp;
    }
};

template <class T>
class Queue {
private:
    Node<T>* rear; // header node 的前一個節點（尾端）
public:
    Queue(int queueCapacity = 10) {
        rear = new Node<T>();
        rear->link = rear; // 初始環狀
    }

    bool IsEmpty() const {
        return rear->link == rear;
    }

    T& Front() const {
        return rear->link->link->data; // header->link = first node
    }

    T& Rear() const {
        return rear->data;
    }

    void Push(const T& item) {
        Node<T>* newNode = new Node<T>(item);
        if (IsEmpty()) {
            // 第一次插入：header->link 指向自己
            rear->link = newNode;
            newNode->link = rear;
        } else {
            newNode->link = rear->link->link; // header->link->link = first node
            rear->link->link = newNode;
        }
        rear = newNode;
    }

    void Pop() {
        if (IsEmpty()) throw runtime_error("Queue is empty");
        Node<T>* header = rear->link;
        Node<T>* frontNode = header->link;
        if (frontNode == rear) {
            // 最後一個元素
            header->link = header;
            rear = header;
        } else {
            header->link = frontNode->link;
        }
        delete frontNode;
    }
};
```

---

## 栈的接口

```java
public interface IStack<T> {
  public boolean push(T item);
  public T pop();
  public T peek();
  public boolean isEmpty();
}
```



## 顺序栈

用数组构建栈

```java
public class ArrayStack<T> implements IStack<T> {
  private T[] array;
  private int count;
  private int capacity;
  
  public ArrayStack(int n) {
    array = (T[]) new Object[n];
    count = 0;
    capacity = n;
  }
  
  //入栈
  @Override
  public boolean push(T item) {
    if (count == capacity) { //满了
      return false;
    }
    array[count] = item;
    count++;
    return true;
  }
  
  //出栈
  @Override
  public T pop() {
    if (isEmpty()) {
      return null;
    }
    count--;
    T tmp = array[count];
    array[count] = null; //help gc
    return tmp;
  }
  
  //查询栈顶
  @Override
  public T peek() {
    if (isEmpty()) {
      return null;
    }
    return array[count-1];
  }
  
  //查询是否为空
  @Override
  public boolean isEmpty() {
    return count == 0;
  }
}
```



## 链式栈

链表实现的栈可以没有长度限制

```java
public class LinkedStack<T> implements IStack<T> {
  private Node<T> top;
  
  @Override
  public boolean push(T item) {
    Node<T> node = new Node(item);
    node.next = top;
    top = node;
    return true;
  }
  
  @Override
  public T pop() {
    if (isEmpty()) {
      return null;
    }
    T tmp = top.val;
    top = top.next;
    return tmp;
  }
  
  @Override
  public T peek() {
    if (isEmpty()) {
      return null;
    }
    return top.val;
  }
  
  @Override
  public boolean isEmpty() {
    return top == null;
  }
  
  private static class Node<T> {
    T val;
    Node next;
    public Node(T val) {
      this.val = val;
    }
  }
}
```




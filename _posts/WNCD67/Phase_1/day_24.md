title: Java x D24
date: 2020-01-08
updated: 
comments: true
tags:
  - learn
  - java
layout: post
---
{% cq %}
<span style="font-variant: small-caps;">homework</span>
ArrayList 源码复写
LinkedList 源码复写
{% endcq %}
<!--more-->
# ArrayList 源码复写
```java
public class MyArrayList {
    private Object[] objs;
    private static final Object[] DEFAULT_EMPTY_LIST = {};
    private static final int DEFAULT_CAPACITY = 10;
    private int size;
    public MyArrayList() {
        objs = DEFAULT_EMPTY_LIST;
    }

    // 增
    public void add(Object object) {
        ensureCapacity(size+1);
        objs[size++]=object;
    }

    private void ensureCapacity(int capacity) {
        if (objs == DEFAULT_EMPTY_LIST) {
            capacity = DEFAULT_CAPACITY;
        }
        if (capacity > objs.length) {
            grow(capacity);
        }
    }

    private void grow(int minCapacity) {
        int newCapacity = objs.length + (objs.length >> 1);

        if (newCapacity < minCapacity) {
            newCapacity = minCapacity;
        }

        Object[] new_objs = new Object[newCapacity];
        System.arraycopy(objs, 0, new_objs, 0, size);
        objs = new_objs;
    }

    // 删
    public void delete(int index) {
        checkRange(index);
        System.arraycopy(objs, index+1, objs, index, size - index - 1);
        objs[--size] = null;
    }

    public void checkRange(int index) {
        if (index < 0 || index > size - 1) {
            throw new IndexOutOfBoundsException("数组下标越界："+index);
        }
    }

    // 查
    public Object get(int index) {
        checkRange(index);
        return objs[index];
    }

    public int size(){
        return size;
    }

    // 改
    public void set(int index, Object object){
        checkRange(index);
        objs[index] = object;
    }

    @Override
    public String toString() {
        StringBuilder sb = new StringBuilder();
        for (int i = 0; i < size; i++) {
//            String sizeStr = "";
//            for (int j = 0; j < (""+size).length() - (""+i).length(); j++){
//                sizeStr += "0";
//            }
//            sizeStr += i;
//            sb.append("{").append(sizeStr).append("}: ").append(objs[i]).append("\n");

            // 格式化字符串，前面补零
            String format = "%0"+(""+size).length()+"d";
            String sizeStr = String.format(format,i);
            sb.append("{").append(sizeStr).append("}: ").append(objs[i]).append("\n");
        }

        return sb.toString();
    }
}
```
# LinkedList 源码复写
```java
public class MyLinkedList {
    private Node head;
    private Node tail;
    private int size;

    public MyLinkedList() {
    }

    public int size() {
        return size;
    }

    public Object get(int index) {
        checkRange(index);
        return find(index).getData();
    }

    private Node find(int index) {
        checkRange(index);
        Node temp;

        if (index > size / 2) {
            temp = tail;
            for (int i = 0; i < size - 1 - index; i++) {
                temp = temp.getPrev();
            }
        } else {
            temp = head;
            for (int i = 0; i < index; i++) {
                temp = temp.getNext();
            }
        }

        return temp;
    }

    public void add(Object object) {
        if (size == 0) {
            addToEmptyList(object);
        } else {
            addLast(object);
        }
    }

    private void addToEmptyList(Object object) {
        Node node = new Node(object);
        head = node;
        tail = node;
        size = 1;
    }

    public void addFirst(Object object) {
        if (size == 0) {
            addToEmptyList(object);
        } else {
            Node newNode = new Node(object);

            head.setPrev(newNode);
            newNode.setNext(head);
            head = newNode;
            size++;
        }
    }

    // addLast
    public void addLast(Object object) {
        if (size == 0) {
            addToEmptyList(object);
        } else {
            Node node = new Node(object);

            tail.setNext(node);
            node.setPrev(tail);
            tail = node;
            size++;
        }
    }

    public void add(int index, Object object) {
        if (index > size || index < 0) {
            throw new IndexOutOfBoundsException();
        } else if (index == size) {
            addLast(object);
        } else if (index == 0) {
            addFirst(object);
        } else {
            Node newNode = new Node(object);
            Node indexNode = find(index);

            indexNode.getPrev().setNext(newNode);
            newNode.setPrev(indexNode.getPrev());

            indexNode.setPrev(newNode);
            newNode.setNext(indexNode);
            size++;
        }
    }

    // removeFirst
    private void removeFirst() {
        head.getNext().setPrev(null);
        head = head.getNext();
        size--;
    }

    // removeLast
    private void removeLast() {
        tail.getPrev().setNext(null);
        tail = tail.getPrev();
        size--;
    }

    public void remove(int index) {
        checkRange(index);
        if (index == 0) {
            removeFirst();
        } else if (index == size - 1) {
            removeLast();
        } else {
            Node node = find(index); // remove Node 要删除的节点

            node.getPrev().setNext(node.getNext());
            node.getNext().setPrev(node.getPrev());
            size--;
        }
    }

    public void set(int index, Object object) {
        checkRange(index);
        Node node = find(index);
        node.setData(object);
    }

    private void checkRange(int index) {
        if (index >= size || index < 0) {
            throw new IndexOutOfBoundsException();
        }
    }
}
```
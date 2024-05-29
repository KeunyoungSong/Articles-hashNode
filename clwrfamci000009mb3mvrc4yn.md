---
title: "Iterator 패턴이란?"
datePublished: Wed May 29 2024 06:05:57 GMT+0000 (Coordinated Universal Time)
cuid: clwrfamci000009mb3mvrc4yn
slug: iterator
tags: iterator-pattern

---

위키 피디아에 따르면,

> In object-oriented programming, the **iterator pattern** is a design pattern in which an iterator is used to traverse a container and access the container's elements. The iterator pattern decouples algorithms from containers; in some cases, algorithms are necessarily container-specific and thus cannot be decoupled.

Iterator 패턴은 iterator가 컨테이너(데이터 구조)와 컨테이너의 요소들을 탐색하기 위해 사용되는 디자인 패턴이다. Iterator는 알고리즘과 컨테이너를 분리한다. 경우에 따라 알고리즘이 필연적으로 컨테이너 한정적인 경우가 있는데 그때에는 분리할 수 없다.

### Iteraotr 패턴의 역할

Decoupling(분리): Iterator 패턴은 알고리즘과 컨테이너를 분리하는 역할을 한다.

- 알고리즘: 데이터를 처리하는 방법이나 절차
- 컨테이너: 데이터를 저장하고 관리하는 구조 (예:배열, 리스트, 트리)

#### 알고리즘과 컨테이너가 분리 가능한 경우

Java 에서 `forEach` 메서드는 리스트, 집합, 맵 등 다양한 컨테이너에서 사용될 수 있다. 이는 iterator 패턴을 통해 각 컨테이너에 대해 같은 방식으로 동작하도록 일반화된다.

```java
List<String> list = Arrays.asList("a", "b", "c");
for (String item : list) {
    System.out.println(item);
}
```



#### 알고리즘이 컨테이너 종속적인 경우

**트리 구조**:  트리에서는 트리의 부모, 자식 노드 등의 관계를 이용해 데이터를 처리한다. 때문에 컨테이너의 요소들을 탐색하기 위한 방법은 트리 구조의 특성이나 필요한 검색 결과에 따라 달라진다

```java
class TreeNode {
    int val;
    TreeNode left;
    TreeNode right;
    
    TreeNode(int x) { val = x; }
}

void inOrderTraversal(TreeNode node) {
    if (node != null) {
        inOrderTraversal(node.left);
        System.out.println(node.val);
        inOrderTraversal(node.right);
    }
}
```


---
https://en.wikipedia.org/wiki/Iterator_pattern

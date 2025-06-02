---
layout  : wiki
title   : Tree(트리) 종류
summary : 헷갈려서 빡쳐서 적음
date    : 2023-07-29 13:16:59 +0900
updated : 2023-07-31 10:15:26 +0900
tag     : 
toc     : true
public  : true
parent  : 
latex   : false
resource: D5BD5038-9F3B-41D3-A42A-14E4ACEEAD90
---
* TOC
{:toc}

# Tree(트리) 종류
위키피디아 기준 트리 용어는 잘 표준화되어 있지 않아 문헌마다 차이가 있다고 한다. 알고리즘 문제나 프로그램 개발에 사용되는 수준과 범위내에서 통용되는 트리에 대해서 정리함.

## Binary(이진) 트리
1. Full Binary Tree 
    - 모든 노드가 0 또는 2개의 자식 노드를 가지는 트리. 다른 표현으로 단말 노드를 제외한 모든 노드가 2개의 자식노드를 가지는 트리.
    - <img src="https://media.geeksforgeeks.org/wp-content/uploads/20221125111700/full.png" alt="gfg-full-bin-tree" width="50%" height="50%" />
<br /><br />
    
2. Perfet Binary Tree
    - 모든 내부노드가 2개의 자식노드를 가지고 있고 모든 단말 노드가 같은 레벨에 있는 트리.
    - 트리 높이가 h라고 할 때, 트리를 구성하는 노드의 개수가 2<sup>h</sup> - 1개가 됨.
    - <img src="https://media.geeksforgeeks.org/wp-content/uploads/20221124094547/perfect.png" alt="gfg-perfect-bin-tree" />

3. Complete Binary Tree
    - 모든 내부노드의 레벨에서 노드들이 가득차있고, 단말노드의 레벨에서는 왼쪽부터 차있는 트리.
    - <img src="https://media.geeksforgeeks.org/wp-content/uploads/20220414154428/complete-200x132.jpg" alt="gfg-complete-bin-tree" />
<br /><br />

4. Balanced Tree
5. AVL Tree
6. Red Black Tree
7. B+ Tree
8. Segment Tree

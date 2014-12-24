## Single Number 

Given an array of integers, every element appears twice except for one. Find that single one.

Note:
Your algorithm should have a linear runtime complexity. Could you implement it without using extra memory?

tags:

* Hash Table 
* Bit Manipulation


```python
def singleNumber(self, A):
    return 2*sum(set(A)) - sum(A)

def singleNumber(self, A):
    return reduce(operator.xor,A)
```

## Maximum Depth of Binary Tree

Given a binary tree, find its maximum depth.

The maximum depth is the number of nodes along the longest path from the root node down to the farthest leaf node.

tags:

* Tree 
* Depth-first Search

```python
def maxDepth(self, root):
        if not root:
            return 0
        return max(self.maxDepth(root.left), self.maxDepth(root.right)) + 1
```

## Same Tree

Given two binary trees, write a function to check if they are equal or not.

Two binary trees are considered equal if they are structurally identical and the nodes have the same value.

tags:

* Tree 
* Depth-first Search

```python
def isSameTree(self, p, q):
    if p == None and q == None:
        return True
    elif p and q :
        return p.val == q.val and self.isSameTree(p.left,q.left) and self.isSameTree(p.right,q.right)
    else :
        return False
```
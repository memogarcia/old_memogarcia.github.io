---
layout: post
title:  "Binary tree data structure"
date:   2017-06-28 13:00:00
categories: algorithm
---
Binary tree

{% highlight python %}

class BinaryNode(object):
    """An object representation of a binary tree node"""
    def __init__(self, value=None):
        self.value = value
        self.left = None
        self.right = None

    def add(self, value):
        if value <= self.value:
            if self.left:
                self.left.add(value)
            else:
                self.left = BinaryNode(value)
        else:
            if self.right:
                self.right.add(value)
            else:
                self.right = BinaryNode(value)

    def delete(self):
        """Remove self from tree
        in case a node is not a leaf, rebalance the tree to get
        the highest element on the right of the tree to be a new
        root node
        """
        if self.left == self.right == None:
            return None

        if self.left is None:
            return self.right

        if self.right is None:
            return self.left

        child = self.left
        grandchild = child.right

        if grandchild:
            while grandchild.right:
                child = grandchild
                grandchild = child.right

            self.value = grandchild.value
            child.right = grandchild.left
        else:
            self.left = child.left
            self.value = child.value

        return self


class BinaryTree(object):
    """An object representation of a binary tree"""
    def __init__(self):
        self.root = None

    def add(self, value):
        if self.root is None:
            self.root = BinaryNode(value)
        else:
            self.root.add(value)

    def contains(self, target):
        node = self.root

        while node:
            if target == node.value:
                return True
            if target < node.value:
                node = node.left
            else:
                node = node.right

        return False

    def remove(self, value):
        if self.root:
            self.root = self.remove_from_parent(self.root, value)

    def remove_from_parent(self, parent, value):
        if parent is None:
            return None

        if value == parent.value:
            return parent.delete()
        elif value < parent.value:
            parent.left = self.remove_from_parent(parent.left, value)
        else:
            parent.right = self.remove_from_parent(parent.right, value)
        return parent


def balanced_tree(ordered_elements):
    """Create a balanced tree from an ordered list"""

    bt = BinaryTree()

    add_range_to_tree(bt, ordered_elements, low=0, high=len(ordered_elements)-1)

    return bt


def add_range_to_tree(binary_tree, ordered_elements, low=0, high=1):
    """Add a range to a binary tree in a way that the tree remains balanced"""
    if low <= high:
        mid = (low+high)/2

        binary_tree.add(mid)
        add_range_to_tree(binary_tree, ordered_elements, low=low, high=mid-1)
        add_range_to_tree(binary_tree, ordered_elements, low=mid+1, high=high)


if __name__ == '__main__':
    bt = BinaryTree()
    bt.add(5)
    bt.add(4)
    bt.add(7)
    print(bt.contains(7))
    bt.remove(7)
    print(bt.contains(7))

    # create a balanced tree from an ordered collection
    ordered = range(10)
    bt = balanced_tree(ordered)
    print(bt.contains(5))



{% endhighlight %}

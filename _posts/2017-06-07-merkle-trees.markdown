---
layout: post
title:  "Merkle Trees in python"
date:   2017-06-06 12:31:00
categories: blockchain
---

Merkle trees are data structures in which each leaf contains a piece of data and each node has a hash computed from its leafs.


But why store data in this way? if the data in a leaf change, it will change the hash of the whole branch and the root as well. so it's a good way to know if the tree has been tempered.

Here is a minimal implementation in python 3.6

{% highlight python %}

import hashlib
from collections import OrderedDict


class MerkleTree(object):
    def __init__(self, transactions=None):
        self.transactions = transactions
        self.previous_transactions = OrderedDict()

    def create(self):
        transactions = self.transactions
        previous_transactions = self.previous_transactions
        temp_transactions = []

        for i in range(0, len(transactions), 2):
            current = transactions[i]

            if i + 1 != len(transactions):
                current_right = transactions[i+1]
            else:
                current_right = ''

            current_hash = hashlib.sha256(current.encode('utf-8'))

            if current_right != '':
                current_right_hash = hashlib.sha256(current_right.encode('utf-8'))

            previous_transactions[transactions[i]] = current_hash.hexdigest()

            if current_right != '':
                previous_transactions[transactions[i+1]] = current_right_hash.hexdigest()

            if current_right != '':
                temp_transactions.append(current_hash.hexdigest() + current_right_hash.hexdigest())
            else:
                temp_transactions.append(current_hash.hexdigest())

        if len(transactions) != 1:
            self.transactions = temp_transactions
            self.previous_transactions = previous_transactions
            self.create()

    def get_previous_transactions(self):
        return self.previous_transactions

    def get_root_leaf(self):
        last_key = list(self.previous_transactions.keys())[-1]
        return self.previous_transactions[last_key]




if __name__ == '__main__':
    print('Merkle trees in action')

    # Truth tree
    truth_merkle = MerkleTree()
    truth_transactions = ['a', 'b', 'c', 'd', 'e']
    truth_merkle.transactions = truth_transactions
    truth_merkle.create()
    truth_previous_transactions = truth_merkle.previous_transactions
    truth_merkle_root = truth_merkle.get_root_leaf()

    # Tempered tree
    tempered_merkle = MerkleTree()
    tempered_transactions = ['a', 'b', 'c', 'd', 'f']
    tempered_merkle.transactions = tempered_transactions
    tempered_merkle.create()
    tempered_previous_transactions = tempered_merkle.previous_transactions
    tempered_merkle_root = tempered_merkle.get_root_leaf()

    # Observe that the root leaf is now different
    print(f'A root transaction {truth_merkle_root}')
    print(f'B root transaction {tempered_merkle_root}')

{% endhighlight %}

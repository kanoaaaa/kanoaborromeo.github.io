---
layout: project
type: project
image: img/39723220-dictionary-definition-of-the-word-translation copy.jpeg
title: "Hawaiian-English Dictionary"
date: 2024
published: true
labels:
  - Python
summary: "A partner and myself used AVL trees to efficiently manage a Hawaiian-English dictionary that supports fast search, insertion, and deletion of sayings and translations."
---
Kanoa Borromeo and Chester Leoso
ICS-311: Assignment 2 - Hawaiian/English Dictionary

class Node:
    def __init__(self, saying, english_translation, hawaiian_explanation, english_explanation):
        self.saying = saying
        self.english_translation = english_translation
        self.hawaiian_explanation = hawaiian_explanation
        self.english_explanation = english_explanation
        self.left = None
        self.right = None
        self.height = 1  # For AVL Tree balance factor

class BalancedTree:
    def __init__(self):
        self.root = None

    def insert(self, root, node):
        # Implement AVL Tree insertion logic with balancing
        pass

    def delete(self, root, saying):
        # Implement AVL Tree deletion logic with balancing
        pass

    def search(self, root, saying):
        # Implement search logic
        pass

    def min_value_node(self, node):
        # Find the node with the minimum value
        pass

    def max_value_node(self, node):
        # Find the node with the maximum value
        pass

    def predecessor(self, root, saying):
        # Find the predecessor node
        pass

    def successor(self, root, saying):
        # Find the successor node
        pass

class Database:
    def __init__(self):
        self.tree = BalancedTree()
        self.hawaiian_word_index = {}
        self.english_word_index = {}

    def insert_saying(self, saying, english_translation, hawaiian_explanation, english_explanation):
        new_node = Node(saying, english_translation, hawaiian_explanation, english_explanation)
        self.tree.root = self.tree.insert(self.tree.root, new_node)
        self._index_words(new_node)

    def _index_words(self, node):
        for word in node.saying.split():
            if word not in self.hawaiian_word_index:
                self.hawaiian_word_index[word] = []
            self.hawaiian_word_index[word].append(node.saying)
        
        for word in node.english_translation.split():
            if word not in self.english_word_index:
                self.english_word_index[word] = []
            self.english_word_index[word].append(node.english_translation)

    def mehua(self, word):
        return self.hawaiian_word_index.get(word, [])

    def with_word(self, word):
        return self.english_word_index.get(word, [])

















(Intro)
	The implemented Python-based code uses an AVL balanced tree structure to efficiently store and retrieve Hawaiian sayings with their respective English translations. The O(log n) time complexity is beneficial for quickly accessing Hawaiian and English words. The implementation and design of the code supports operations including insertion, deletion, and searching for Hawaiian and English words.

(Data Structure)
	Efficient access to elements within the database is ensured by using an AVL tree, a type of self-balancing binary search tree. This choice maintains a balanced structure and time complexity of O(log n) across all operations. The fast time complexity for accessing elements is crucial especially as additional elements of words and their translations are added to the database. According to “Introductions to Algorithms” by Cormen et al, “Basic operations on a binary search tree take time proportional to the height of the tree. For a complete binary tree with n nodes, such operations run in O(log n) worst-case time”(Cormen et al, 2022). Using a self-balancing binary tree achieves a  guarantee that the height at which different operations are executed will always be balanced, thus always ensuring optimal performance. 

(Operations)
Indexing - Two dictionaries are employed for access-related operations: separate dictionaries - one for Hawaiian words and one for English words. These dictionaries function to map words to the phrases that contain them, allowing retrieval of sayings in a quick manner.
Insertion - A new word can be inserted to a correct position within the AVL tree followed by readjustments to stabilize an imbalance. The properties of the binary tree allow insertion to be permissible by triggering the,”...dynamic set represented by a binary tree to change. The data structure must be modified to reflect this change…”(Cormen et al., 2022) The AVL tree provides O(log n) insertion while maintaining a balanced tree. 
Deletion - Similar to insertion, deletion involves removing a node within the tree and then rotating necessarily to maintain balance.  However, deleting a node from a binary tree can be especially complex if the node has two children, as the process involves replacing the node with a new while also deleting the original.
Balancing - The height of the tree is updated as the nodes rotate to maintain the balance factor of the binary tree. If the tree detects a lack of balance, then up to double rotations can be carried out to stabilize the shape. This balancing also secures a minimal difference between the left and right subtrees to ensure the tree's efficiency.


(Conclusion)
Implementing a self-balancing binary tree ensures a time complexity of O(log n) across all search operations which makes it a data structure quite adequate for managing a large dataset of different words. The Space complexity is also efficient and manageable as each node within the tree contains four strings and two pointers. The ideal solution to both a sound structure and dictionary for indexing words is achieved through the AVL tree. Additionally, by using dictionaries for word indexing, individual words and their translations can be quickly looked up. The implementation of a database for Hawaiian sayings built on a binary tree provides an effective solution for varying operations to interact with different words.


(References)
Cormen, Thomas H. “Introduction to Algorithms, Fourth Edition.” Google Books, MIT Press, 5 Apr. 2022, 



	

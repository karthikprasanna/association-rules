# Justification for choosing FP-Growth

- Efficiency: The FP-Tree approach is generally more efficient than Apriori, especially when dealing with large datasets. It reduces the need for repeated database scans, making it faster for frequent pattern mining.

- Reduced Candidate Generation: Apriori generates a large number of candidate itemsets, many of which may not be frequent. In contrast, the FP-Tree approach significantly reduces candidate generation by compactly storing transactions and using a tree structure.

- Memory Efficiency: FP-Tree often requires less memory because it compresses the dataset into a structured tree, which is more memory-efficient than maintaining a large list of itemsets as Apriori does.

- Single Scan: FP-Tree requires only a single pass over the dataset to build the tree and count item frequencies, making it more suitable for situations where multiple database scans are costly or not feasible.

- Reduced Computation: The Apriori algorithm generates and checks many candidate itemsets, leading to higher computational overhead. The FP-Tree approach reduces the number of itemsets to be considered.

- Easier Rule Generation: FP-Tree often simplifies the process of generating association rules since it maintains direct information about the support of individual itemsets.

# `FP-Growth` Algorithm

# Ordered Itemset Generation

Filter the dataset with a given minimum support threshold and generate a list of itemsets in descending order of support.

# Trie Data Structure

The code implements a Trie data structure. A Trie, also known as a Prefix Tree, is a tree-like data structure used to efficiently store and retrieve a dynamic set of strings. Each node in the Trie represents a character, and the path from the root to a particular node forms a string.

## `TrieNode` Class

### Constructor `__init__(self)`

- Initializes a `TrieNode` object.
- Attributes:
  - `char`: The character associated with the node. Default value is -1.
  - `count`: The count of words that pass through this node. Default value is 0.
  - `children`: A list of child nodes, each representing a character. Initially empty.

## `root` Variable

- Initializes the root of the Trie as an instance of the `TrieNode` class. This root serves as the starting point for inserting and searching words in the Trie.

## `insert(word)` Function

### Parameters:

- `word` (str): The word to be inserted into the Trie.

### Operation:

- Inserts a word into the Trie by iterating through each character in the word and creating the necessary nodes as needed.
- If a character already exists as a child of the current node, it increments the count for that character's node.
- If a character does not exist as a child of the current node, it creates a new node and appends it to the list of children, updating the count.
- The `root` variable always remains the starting point for each insertion.

# DFS Function for Trie Pattern Matching

The code defines a Depth-First Search (DFS) function used for pattern matching in a Trie data structure. This function traverses the Trie and generates conditional pattern bases for a specific item.

## Function: `dfs(node, pattern, item, patBase)`

### Parameters:

- `node` (TrieNode): The current node being explored in the Trie.
- `pattern` (list): A list of characters representing the current pattern being constructed.
- `item` (int): The item (character) for which conditional pattern bases are generated.
- `patBase` (dict): A dictionary that stores the conditional pattern bases.

### Operation:

- The `dfs` function recursively explores the Trie to construct conditional pattern bases for a specific item.
- When the current node's character matches the specified item, the function creates a conditional pattern base for the pattern constructed so far.
- The conditional pattern base is stored in the `patBase` dictionary with the pattern as a tuple key and the count as the value.

# Conditional FP-Tree Construction

The code constructs conditional FP-trees based on conditional pattern bases derived from a Trie data structure. FP-trees are a data structure used for frequent pattern mining, and the code builds these trees for each conditional pattern base.

## Conditional FP-Tree Construction

### Variables:

- `conditional_fp_trees` (list): A list used to store conditional FP-trees.

### Operation:

- The code iterates through each item in the `condPatBase` dictionary, where each item represents a movie and its corresponding conditional pattern base.
- For each conditional pattern in the pattern base, it calculates the total count by summing up the support values (counts).
- It then identifies the intersection of patterns by comparing the patterns and stores the result in the `intersection_list` variable.
- If the intersection list is not empty, it constructs a conditional FP-tree entry for the movie with the intersection pattern and its total count.
- The conditional FP-tree entry is added to the `conditional_fp_trees` list.

### Parameters:

- `s` (iterable): The input set from which subsets are generated.
- `n` (int): The size of subsets to be generated.

### Operation:

- The `findsubsets` function uses itertools to generate all combinations of `n` elements from the input set `s`.
- It returns the list of subsets as a list of tuples.

# Frequent Pattern Itemsets Generation

### Variables:

- `frequent_pattern_itemsets` (list): A list used to store the generated frequent pattern itemsets.

### Operation:

- The code iterates through each conditional FP-tree in the `conditional_fp_trees` list.
- For each conditional FP-tree, it extracts movie IDs and their associated patterns.
- It then generates all possible subsets of these patterns for different lengths (from 1 to the length of the pattern).
- Each subset is combined with the movie ID and its count to form a frequent pattern itemset.
- These itemsets are added to the `frequent_pattern_itemsets` list.

# Association Rules Generation

### Variables:

- `association_rules` (list): A list used to store the generated association rules.

### Operation:

- The code iterates through each frequent itemset in the `frequent_pattern_itemsets` list.
- For each itemset, it extracts the individual items and their counts.
- It calculates the confidence for each itemset by dividing the count by the support (frequency) of the item in a given dataset.
- An association rule is constructed with the following components:
  - "X": The item being analyzed (antecedent).
  - "Y": The remaining items in the itemset (consequent).
  - "support": The support (count) of the itemset.
  - "confidence": The confidence of the rule.
- Each association rule is added to the `association_rules` list.

## Sorting Association Rules

### Operation:

- The code performs two sorting operations on the generated association rules:
  - Sorts the association rules by support in descending order to find the top 100 rules based on support.
  - Sorts the association rules by confidence in descending order to find the top 100 rules based on confidence.

# Association Rules Evaluation Documentation

## Variables and Initialization:

- `avg_recall` (float): Initialized to 0.0, it stores the cumulative average recall.
- `avg_precision` (float): Initialized to 0.0, it stores the cumulative average precision.
- `rule_num` (list): A list that contains the range of numbers of rules to be considered (e.g., [1, 2, 3, ..., 10]).
- `avg_recall_list` (list): A list to store average recall values for each number of rules.
- `avg_precision_list` (list): A list to store average precision values for each number of rules.

## Calculation of Average Recall and Average Precision

### Operation:

- The code iterates through each number of rules specified in `rule_num`.
- For each number of rules, it iterates through each row in the `test_df`, which contains test data.
- For each user in the test data, it retrieves their test movies and the movies they have rated above 2 in the training data.
- It generates a recommendation set by considering the top `num` association rules that match the movies in the training set.
- It calculates recall as the ratio of the number of recommended movies that were also in the test set to the size of the test set.
- It calculates precision as the ratio of the number of recommended movies that were also in the test set to the size of the recommendation set.
- The cumulative average recall and precision are updated using a moving average formula.
- The average recall and precision for the current number of rules are appended to the respective lists.

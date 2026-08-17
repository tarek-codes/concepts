


---

## B-Trees vs. B+ Trees in Database Indexing

**B-Trees** and **B+ Trees** are self-balancing tree data structures used to store sorted data and allow searches, sequential access, insertions, and deletions in logarithmic time. They are the foundation of almost all modern database indexing systems.

### Key Differences

1.  **Data Storage**:
    *   **B-Tree**: Data (keys and values) is stored in both internal (non-leaf) nodes and leaf nodes.
    *   **B+ Tree**: Only keys are stored in internal nodes for routing. All actual data (keys and values/pointers) is stored exclusively in the leaf nodes.

2.  **Leaf Node Linking**:
    *   **B-Tree**: Leaf nodes are not necessarily linked. Sequential scans require traversing the tree structure repeatedly.
    *   **B+ Tree**: Leaf nodes are linked together in a doubly linked list. This allows for extremely efficient range queries and full table scans by traversing the linked list at the bottom level.

3.  **Height and Fan-out**:
    *   Because B+ Trees store only keys in internal nodes, they can fit more keys per node (higher fan-out). This results in a shorter tree height for the same amount of data, reducing the number of disk I/O operations required to reach a leaf node.

### Practical Example

Consider a database table with 1 billion rows.

*   **Point Query (e.g., `SELECT * WHERE id = 123`)**:
    Both trees perform similarly well, requiring $O(\log n)$ disk accesses. However, the B+ Tree might be slightly faster due to its lower height.

*   **Range Query (e.g., `SELECT * WHERE id BETWEEN 100 AND 200`)**:
    *   **B-Tree**: The database must traverse down to find the first key (100), then traverse back up and down to find subsequent keys. This is inefficient.
    *   **B+ Tree**: The database finds the starting key (100) in a leaf node, then simply follows the linked list pointers to the right until it reaches key 200. This is a linear scan of the leaf nodes, which is highly optimized for disk I/O.

### When to Use Which?

*   **Use B+ Trees** for most database applications, especially those involving range queries, sorting, or large datasets. This is why MySQL (InnoDB), PostgreSQL, and Oracle use B+ Trees for their primary indexes.
*   **Use B-Trees** when memory is constrained and you need to minimize node count, or in file systems where metadata is stored in internal nodes (e.g., BSD's UFS).

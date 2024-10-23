# Lock Coupling

- either B-Tree or ART
- copy old code, add `std::shared_mutex`

# in B-tree

- empty tree: root is nullptr
- protect root: have additional lock, or dummy node
- nodes have locks
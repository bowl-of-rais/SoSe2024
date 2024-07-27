one! ssh key in master branch

----


```cpp
// old insert

// inner nodes
if (inner->isFull()) {  
    // Split inner eagerly if you want to  
    // avoid backtracking    Key splitKey = inner->keyAt(inner->size() / 2);  
    InnerNode* right = &inner->split(splitKey);  
    if (!parent) {  
        // splitting root: make new root to insert split parts into  
        auto* newRoot = new InnerNode(splitKey, node, right);  
        root = newRoot;  
        inner = static_cast<InnerNode*>(newRoot->valueAt(newRoot->lowerBound(key)));  
    } else {  
        Index parent_bound = parent->lowerBound(splitKey);  
        // splitting non-root inner node: insert in parent  
        parent->insert(parent_bound, right);  
        parent->insert(splitKey, inner);  
        // find correct part of split  
        // TODO: optimize, directly compare key with splitKey  
        inner = static_cast<InnerNode*>(parent->valueAt(parent->lowerBound(key)));  
    }  
}

// leaf nodes
if (leaf->isFull()) {  
    // Leaf is full, split it  
    Key splitKey = leaf->keyAt(leaf->size() / 2);  
    LeafNode* right = &leaf->split(splitKey);  
    if (!parent) {  
        // splitting root: make new root to insert split parts into  
        std::cout << "making new root!\n";  
        auto* newRoot = new InnerNode(splitKey, leaf, right);  
        root = newRoot;  
        leaf = static_cast<LeafNode*>(newRoot->valueAt(newRoot->lowerBound(key)));  
    } else {  
        Index parent_bound = parent->lowerBound(splitKey);  
        // splitting non-root leaf node: insert in parent  
        parent->insert(parent_bound, right);  
        parent->insert(splitKey, leaf);  
        // find correct part of split  
        leaf = static_cast<LeafNode*>(parent->valueAt(parent->lowerBound(key)));  
    }  
    assert(leaf);  
}

```
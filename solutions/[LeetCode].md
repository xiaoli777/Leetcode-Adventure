**Solution:**
The basic idea is to build a tree `lockingTree`.

```cpp
struct lockingTree {
    int user;
    lockingTree* p; // its parent
    vector<lockingTree*> child;
    lockingTree(int u, lockingTree* par) : user(u), p(par) {}
};
```

Then, we use a hashmap `cache` to store treenode, so we can use `num` to find the `lockingTree` we want in the hashmap.

- For lock(), we use `num` to find the `lockingTree`, and check if it's locked.
- For unlock(), we use `num` to find the `lockingTree`, and check if it's locked by the same user.
- For upgrade(), we use `num` to find the `lockingTree`, then check if its parent or children are locked. If we pass the check process, then we use DFS to upgrade.



**Code:**

```cpp
struct lockingTree {
    int user;
    lockingTree* p;
    vector<lockingTree*> child;
    lockingTree(int u, lockingTree* par) : user(u), p(par) {}
};

class LockingTree {
public:
    lockingTree* root;
    unordered_map<int, lockingTree*> cache;
    LockingTree(vector<int>& parent) {
        int n = parent.size();
        root = new lockingTree(-1, nullptr);
        cache[-1] = root;
        
        // generate treenode
        for(int i = 0; i < n; i++) {
            lockingTree* cur = new lockingTree(-1, nullptr);
            cache[i] = cur;
        }
        
        // bind a child to its parent
        for(int i = 0; i < n; i++) {
            cache[i]->p = cache[parent[i]];
            cache[parent[i]]->child.push_back(cache[i]);
        }
    }
    
    bool lock(int num, int user) {
        if(cache[num]->user != -1)
            return false;
        cache[num]->user = user;
        return true;
    }
    
    bool unlock(int num, int user) {
        if(cache[num]->user != user)
            return false;
        cache[num]->user = -1;
        return true;
    }
    
    bool upgrade(int num, int user) {
        lockingTree* cur = cache[num];
        // check parent
        while(cur != nullptr) {
            if(cur->user != -1)
                return false;
            cur = cur->p;
        }
        
        // check children
        cur = cache[num];
        if(!check(cur))
            return false;
        
        // update users
        cur = cache[num];
        update(cur);
        cur->user = user;
        return true;
    }
    
    bool check(lockingTree* cur) {
        for(auto &child : cur->child) {
            if(child->user != -1)
                return true;
            
            if(check(child))
                return true;
        }
        return false;
    }
    
    void update(lockingTree* cur) {
        for(auto &child : cur->child) {
            child->user = -1;
            update(child);
        }
    }
};
```

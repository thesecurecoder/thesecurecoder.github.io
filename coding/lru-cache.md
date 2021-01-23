## LRU Cache

[Problem statement](https://leetcode.com/problems/lru-cache).

### Thought process

* Hashmap to store Cache values.
* Doubly linked list to track access to cache units. 
* Hashmap to map key to cache unit.
* Get operation:
  * Get value from cache.
  * Update access tracker by removing corresponding cache unit from linked list and adding it to the end. Update KRU unit and MRU unit accordingly.
* Put operation:
  * Check if key already exists. If yes, update value and access tracker by removing corresponding cache unit from linked list and adding it to the end. Update KRU unit and MRU unit accordingly.
  * Check if size is exceeding capacity. If yes, remove LRU unit and corresponding cache entry.
  * Add new entry with value to cache. Add new entry to the rear end of the linked list to track it as the MRU unit.
* **Time complexity:**
  * GET: O(1)
  * PUT: O(1)
* **Space complexity:** O(N) [N = capacity]

### Enough talk, show me the code

```java
class LRUCache {
    
    private HashMap<Integer,Integer> cache; // Cache implementation using hashmap // O(1) read and write assumed
    
    private HashMap<Integer, Cache> cacheMap; // Cache map // Map of key to cache unit
    
    private Cache lruUnit; // Least recently used cache unit
    
    private Cache mruUnit; // Most recently used cache unit
    
    private int capacity; // Cache capacity;

    public LRUCache(int capacity) {
        this.cache = new HashMap<Integer,Integer>();
        this.cacheMap = new HashMap<Integer,Cache>();
        this.lruUnit = null;
        this.mruUnit = null;
        this.capacity = capacity;
    }
    
    public int get(int key) {
        if (cacheMap.get(key) != null) {
            updateAccessToUnit(key);
            debug();
            return cache.get(key);
        }
        return -1;
    }
    
    public void put(int key, int value) {
        if (cacheMap.get(key) != null) {
            updateAccessToUnit(key);
            cache.remove(key);
            cache.put(key,value);
        } else {
            addUnit(key, value);
        }
        debug();
    }
    
    private void updateAccessToUnit(int key) {
        Cache cache = cacheMap.get(key);
        Cache lastCache = cache.lastCache;
        Cache nextCache = cache.nextCache;
        if(lastCache != null) {
            lastCache.nextCache = nextCache;
            if (nextCache != null) {
                nextCache.lastCache = lastCache;
            } else {
                mruUnit = lastCache;
            }
        } else {
            lruUnit = nextCache;
            if (nextCache != null) {
                nextCache.lastCache = null;
            }
        }
        mruUnit.nextCache = cache;
        cache.lastCache = mruUnit;
        cache.nextCache = null;
        mruUnit = cache;
        if (lruUnit == null) {
            lruUnit = mruUnit;
        }
        debug();
    }
    
    private void addUnit(int key, int value) {
        if(cacheMap.size() == capacity) {
            cacheMap.remove(lruUnit.key);
            if(lruUnit.nextCache != null) {
                lruUnit = lruUnit.nextCache;
                lruUnit.lastCache = null;
            } else {
                lruUnit = null;
                mruUnit = null;
            }
        }
        Cache cache = new Cache(key, null, mruUnit);
        cacheMap.put(key, cache);
        this.cache.put(key,value);
        if(mruUnit != null) {
            mruUnit.nextCache = cache;
        }
        mruUnit = cache;
        if(lruUnit == null) {
            lruUnit = mruUnit;
        }
    }
    
    private void debug() {
        System.out.println("----------\n[DEBUG] LRU - " + lruUnit.key);
        System.out.println("[DEBUG] MRU - " + mruUnit.key + "\n----------\n");
    }
}

// Cache unit
class Cache {
    
    public int key;
        
    public Cache nextCache;
    
    public Cache lastCache;
    
    public Cache(int key, Cache c1, Cache c2) {
        this.key = key;
        this.nextCache = c1;
        this.lastCache = c2;
    }
    
}
```
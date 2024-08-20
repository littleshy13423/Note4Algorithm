# LRU C++ 实现

```C++
#include <iostream>
#include <unordered_map>
#include <list>

class LRUCache {
private:
    int capacity;
    std::list<int> keys;  // 双向链表，存储访问顺序
    std::unordered_map<int, std::pair<int, std::list<int>::iterator>> cache;  // 哈希表，存储键值对和对应的链表迭代器

public:
    LRUCache(int capacity) : capacity(capacity) {}

    int get(int key) {
        auto it = cache.find(key);
        if (it == cache.end()) {
            return -1;  // 如果键不存在，返回-1
        } else {
            // 移动访问的键到链表头部
            keys.erase(it->second.second);
            keys.push_front(key);
            it->second.second = keys.begin();
            return it->second.first;
        }
    }

    void put(int key, int value) {
        auto it = cache.find(key);
        if (it != cache.end()) {
            // 如果键已经存在，更新值并将其移到链表头部
            keys.erase(it->second.second);
            keys.push_front(key);
            it->second = {value, keys.begin()};
        } else {
            // 如果键不存在
            if (keys.size() >= capacity) {
                // 如果缓存已满，移除链表尾部的最久未使用的元素
                int old_key = keys.back();
                keys.pop_back();
                cache.erase(old_key);
            }
            // 插入新键值对
            keys.push_front(key);
            cache[key] = {value, keys.begin()};
        }
    }
};
```

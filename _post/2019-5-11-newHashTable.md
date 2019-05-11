### New Improvements to Hash Table Performance

首先介绍一下c++中的四种数据结构的查找功能的实现：

1. std::unordered_map<int, int>
unordered_map内部实现了一个哈希表，因此其元素的排列顺序是杂乱的，无序的。
哈希表的特性是牺牲了大量的内存，可以较快的实现查找、存储等操作。通过牺牲空间的方法来换查找效率。
```
解决哈希冲突的方法：
- 开放寻址法（Open Addressing)
	* 二次查找(Quadratic Probing) 按顺序检查s + 1^2, s - 1^2, s + 2^2, s - 2^2...
	* 二度哈希(Rehashing/double hashing)，添加时第一次选用hash1函数，如果被占用则选用hash2函数再次hash直至找到空位置
- 链接技术 (chaining)
	* 把哈希到同一个槽中的元素放到一个链表中
```

2. std::map<int, int>
map内部实现了一个红黑树，该结构具有自动排序的功能，因此map内的所有元素都是有序的，红黑树的每一个节点都代表着map的一个元素，因此，map对于查找、删除、添加等功能都相当于对红黑书进行这样的操作。

回忆一下红黑树的性质：
```
- 节点必须是红色或者黑色。
- 根节点必须是黑色。
- 叶节点(NIL)是黑色的。（NIL节点无数据，是空节点）
- 红色节点必须有两个黑色儿子节点。
- 从任一节点出发到其每个叶子节点的路径，黑色节点的数量是相等的。

这些属性是的红黑树具有一个关键的属性：从根节点到最远的叶子节点的路径长与到最近的叶子节点的路径长度不会相差超过2。
每次插入元素时，元素将会被设置为红色节点。
因此红黑树查找的速率基本可以保持O(logn)级别。

```
3. boost::flat_map<int, int>
这是一种基于矢量地图的实现

4. linear search
线性搜索，根据顺序一个个查找

首先看一下这四种方法在不同数据量下的表现：


#### Optimization 1/7:
#### Optimization 2/7: 
#### Optimization 3/7:
#### Optimization 4/7:
#### Optimization 5/7:
#### Optimization 6/7:
#### Optimization 7/7:

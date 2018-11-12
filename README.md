# BdBloomFilter
A java solution that resolved big data deduplication problem

-------

BloomFilter介绍：Bloom filter 是由 Howard Bloom 在 1970 年提出的二进制向量数据结构，它具有很好的空间和时间效率，被用来检测一个元素是不是
集合中的一个成员。查询返回“可能在集合中”或“绝对不在集合中”。元素可以添加到集合中，但不会被删除（尽管可以通过“计数”过滤器来解决）; 添加到集合
中的元素越多，误报的概率就越大。
https://en.wikipedia.org/wiki/Bloom_filter    https://baike.baidu.com/item/bloom%20filter/6630926

目的：解决频繁接收重复数据造成的后台(持久层)压力问题

实现：redis bitMap数据类型、guava murmurHash算法

实现原理：借鉴并重构google guava的bloomFilter实现(ps:guava的实现是基于long，而bdBloomFilter是基于redis bitmap结构)，
通过murmurhash3函数生成位节点，并加了自己的可配置参数

优点：相较于数据库层面校验速度快得多、节省大量内存、相较于guava的实现BdBloomFilter不会把jvm搞大

缺点：会有误判率(但是控制的好基本不会出现问题)、放入过滤器里的数据不好删除(这意味着 要过滤掉的数据突然不用过滤了，这样的场景我还没有想到)

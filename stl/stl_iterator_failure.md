# STL容器迭代器失效问题总结

## vector

vector在insert新元素时，会先使用备用空间，如果备用空间大小不足够插入所有新元素，则vector会先将长度扩大并重新分配空间，
意味着vector所占用的连续地址空间将会改变，此时之前在vector上进行操作的迭代器将失效
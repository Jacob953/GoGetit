# 8. Search - 搜索

- [8. Search - 搜索](#8-search---搜索)
	- [Go - 广度优先搜索](#go---广度优先搜索)
	- [Go - 深度优先搜索](#go---深度优先搜索)
	- [Go - 搜索算法的应用](#go---搜索算法的应用)

搜索算法往往是基于“图”这种数据结构的，如果你对图的存储方式还有疑惑，建议再阅读一下 [数据结构](../../Data-Structure/README.md) 章节

本节将会使用邻接表的图存储方式，向你介绍最常见的两种搜索算法：BFS 和 DFS

## Go - 广度优先搜索

实现步骤：

```Golang
//search path by BFS
func (self *Graph) BFS(s int, t int) {

	//todo
	if s == t {
		return
	}

	//init prev
	prev := make([]int, self.v)
	for index := range prev {
		prev[index] = -1
	}

	//search by queue
	var queue []int
	visited := make([]bool, self.v)
	queue = append(queue, s)
	visited[s] = true
	isFound := false
	for len(queue) > 0 && !isFound {
		top := queue[0]
		queue = queue[1:]
		linkedlist := self.adj[top]
		for e := linkedlist.Front(); e != nil; e = e.Next() {
			k := e.Value.(int)
			if !visited[k] {
				prev[k] = top
				if k == t {
					isFound = true
					break
				}
				queue = append(queue, k)
				visited[k] = true
			}
		}
	}
```

## Go - 深度优先搜索

实现步骤：



```Golang
//search by DFS
func (self *Graph) DFS(s int, t int) {

	prev := make([]int, self.v)
	for i := range prev {
		prev[i] = -1
	}

	visited := make([]bool, self.v)
	visited[s] = true

	isFound := false
	self.recurse(s, t, prev, visited, isFound)

	printPrev(prev, s, t)
}

//recursivly find path
func (self *Graph) recurse(s int, t int, prev []int, visited []bool, isFound bool) {

	if isFound {
		return
	}

	visited[s] = true

	if s == t {
		isFound = true
		return
	}

	linkedlist := self.adj[s]
	for e := linkedlist.Front(); e != nil; e = e.Next() {
		k := e.Value.(int)
		if !visited[k] {
			prev[k] = s
			self.recurse(k, t, prev, visited, false)
		}
	}
}
```

## Go - 搜索算法的应用

搜索算法的应用途径非常常见，譬如地图中肯定会涉及的最短路径推荐

但并不是所有的搜索算法都适用于最短路径的，上文所述的 DFS 就无法满足最短路径的要求，但 BFS 可以

搜索算法的具体方法还有很多，比如即将在进阶篇中提到的 A*
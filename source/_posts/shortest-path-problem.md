---
title: shortest path problem
date: 2023-06-01 16:16:03
tags: 
---



```python
def dij_adjacency_matrix_without_heap(adj_matrix, source):
    dist = [float('inf')] * len(adj_matrix)
    visited = [False] * len(adj_matrix)
    dist[source] = 0

    for i in range(len(adj_matrix)):
        min_dist = float('inf')
        min_index = -1
        for j in range(len(adj_matrix)):
            # find the min distance node in the unvisited nodes
            # this can be optimized by using heap
            # source is the first node
            if not visited[j] and dist[j] < min_dist:
                min_dist = dist[j]
                min_index = j
                
        # if there is no unvisited node, break
        # or if the min distance node is not reachable, break
        if min_index == -1:
            break
        
        # mark the min distance node as visited
        visited[min_index] = True
        
        # update the distance of the unvisited nodes
        for j in range(len(adj_matrix)):
            if not visited[j] and adj_matrix[min_index][j] != float('inf') and \
                    dist[min_index] + adj_matrix[min_index][j] < dist[j]:
                dist[j] = dist[min_index] + adj_matrix[min_index][j]

    return dist
```



```python
def dij_adjacency_matrix(adj_matrix, source):
    dist = [float('inf')] * len(adj_matrix)
    visited = [False] * len(adj_matrix)
    dist[source] = 0

    # use the heap (priority queue) to store the distance between the visited node and unvisited node
    heap = []
    heapq.heappush(heap, (0, source))

    while heap:
        # find the min distance node
        distance, cur = heapq.heappop(heap)

        # update the distance of the unvisited nodes
        for neighbor in range(len(adj_matrix[cur])):
            if not visited[neighbor] and distance + adj_matrix[cur][neighbor] < dist[neighbor]:
                dist[neighbor] = dist[cur] + adj_matrix[cur][neighbor]
                heapq.heappush(heap, (dist[neighbor], neighbor))

        # mark the min distance node as visited
        visited[cur] = True

    return dist
```



```python
def dij_adjacency_list(adj_list, source):
    dist = [float('inf')] * len(adj_list)
    visited = [False] * len(adj_list)
    dist[source] = 0

    # use the heap (priority queue) to store the distance between the visited node and unvisited node
    heap = []
    heapq.heappush(heap, (0, source))

    while heap:
        # find the min distance node
        distance, cur = heapq.heappop(heap)
        
        # update the distance of the unvisited nodes
        for neighbor, weight in adj_list[cur]:
            if not visited[neighbor] and distance + weight < dist[neighbor]:
                dist[neighbor] = dist[cur] + weight
                heapq.heappush(heap, (dist[neighbor], neighbor))
                
        # mark the min distance node as visited
        visited[cur] = True

    return dist
```




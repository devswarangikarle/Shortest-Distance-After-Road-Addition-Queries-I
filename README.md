# Shortest-Distance-After-Road-Addition-Queries-I

You are given an integer n and a 2D integer array queries.

There are n cities numbered from 0 to n - 1. Initially, there is a unidirectional road from city i to city i + 1 for all 0 <= i < n - 1.

queries[i] = [ui, vi] represents the addition of a new unidirectional road from city ui to city vi. After each query, you need to find the length of the shortest path from city 0 to city n - 1.

Return an array answer where for each i in the range [0, queries.length - 1], answer[i] is the length of the shortest path from city 0 to city n - 1 after processing the first i + 1 queries.

class Solution:
    def updateDistances(self, graph, current, distances):
        newDist = distances[current] + 1
        
        for neighbor in graph[current]:
            if distances[neighbor] <= newDist:
                continue
                
            distances[neighbor] = newDist
            self.updateDistances(graph, neighbor, distances)
    
    def shortestDistanceAfterQueries(self, n: int, queries: List[List[int]]) -> List[int]:
        distances = [n - 1 - i for i in range(n)]
        
        graph = [[] for _ in range(n)]
        for i in range(n-1):
            graph[i + 1].append(i)
        
        answer = []
        
        for source, target in queries:
            graph[target].append(source)
            distances[source] = min(distances[source], distances[target] + 1)
            self.updateDistances(graph, source, distances)
            
            answer.append(distances[0])
        
        return answer

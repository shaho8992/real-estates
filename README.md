

graph = {
    'A': {'B': 10, 'C': 6, 'D': 3},
    'B': {'E': 5, 'F': 3},
    'C': {'G': 3, 'H': 4},
    'D': {'H': 5},
    'E': {},
    'F': {},
    'G': {},
    'H': {}
}

graph2 = {
    'A': {'B': 20, 'D': 30},
    'B': {'A': 20, 'C': 25, 'D': 70},
    'D': {'A': 30, 'B': 70, 'F': 15, 'G': 20, 'E': 35},
    'C': {'B': 25, 'E': 40},
    'E': {'D': 35, 'C': 40, 'G': 50, 'H': 70},
    'F': {'D': 15, 'G': 10},
    'G': {'F': 10, 'D': 20, 'E': 50, 'H': 60},
    'H': {'E': 70, 'G': 60}
}

# Heuristic Definitions
Heuristic = {'A': 8, 'B': 8, 'C': 2, 'D': 7, 'E': 3, 'F': 6, 'G': 0, 'H': 3}
Heuristic2 = {'A': 90, 'B': 95, 'C': 88, 'D': 77, 'E': 50, 'F': 55, 'G': 50, 'H': 0}


def A_star(graph, start, goal, h):
    parent = dict()
    frontier = {start:h[start]}
    visited = {start:h[start]}
    while frontier:
        sorted_queue = sorted(frontier, key=frontier.get) #sorted_queue is a list
        node = sorted_queue[0]
        g = frontier.pop(node)-h[node]
        if node == goal:
            return parent
        for n in graph.get(node):
            if n not in visited:
                F = h[n] + graph[node][n]+g
                visited[n] = F
                frontier[n] = F
                parent[n] = node
            else:
                F = h[n] + graph[node][n]+g
                if visited[n] >= F:
                    visited[n] = F
                    frontier[n] = F
                    parent[n] = node
                    

                    

def traceback(parent, start, goal):
    path = [goal]
    while path[-1] != start:
        path.append(parent[path[-1]])
    path.reverse()
    return path


# Test Case 1
parent1 = A_star(graph, 'A', 'G', Heuristic)
path1 = traceback(parent1, 'A', 'G')
print("Path from A to G:", path1)

# Test Case 2
parent2 = A_star(graph2, 'A', 'H', Heuristic2)
path2 = traceback(parent2, 'A', 'H')
print("Path from A to H:", path2)
-----------------------------------------------------------------------------

///greedy
def GBFS(graph, start, goal, h):
    parent = dict()
    frontier = {start: h[start]}  # Only consider the heuristic value
    visited = set([start])  # Track visited nodes
    
    while frontier:
        # Sort the frontier by heuristic value and choose the node with the smallest heuristic
        sorted_queue = sorted(frontier, key=frontier.get)  # Sort by heuristic value
        node = sorted_queue[0]
        frontier.pop(node)  # Remove the node with the smallest heuristic
        
        if node == goal:  # Goal found
            return parent
        
        for n in graph.get(node, {}):  # Iterate over neighbors
            if n not in visited:
                visited.add(n)
                frontier[n] = h[n]  # Add the neighbor to the frontier with its heuristic value
                parent[n] = node  # Update the parent of the neighbor

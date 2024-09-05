def uniformCostSearchAlgo(start, end):
    active_set = set([start])
    visited_set = set()
    distances = {start: 0}  
    path_map = {start: start}  

    while active_set:
        current_node = None

        for node in active_set:
            if current_node is None or distances[node] < distances[current_node]:
                current_node = node

        if current_node == end:
            path = []
            while path_map[current_node] != current_node:
                path.append(current_node)
                current_node = path_map[current_node]
            path.append(start)
            path.reverse()
            print(f"Optimal Path: {path}")
            return path

        if current_node is None:
            print("No possible path found!")
            return None

        active_set.remove(current_node)
        visited_set.add(current_node)

        for (neighbor, cost) in retrieveNeighbors(current_node):
            if neighbor not in active_set and neighbor not in visited_set:
                active_set.add(neighbor)
                path_map[neighbor] = current_node
                distances[neighbor] = distances[current_node] + cost
            elif distances[neighbor] > distances[current_node] + cost:
                distances[neighbor] = distances[current_node] + cost
                path_map[neighbor] = current_node
                if neighbor in visited_set:
                    visited_set.remove(neighbor)
                    active_set.add(neighbor)

    print("No possible path found!")
    return None

def retrieveNeighbors(node):
    return graph_nodes[node] if node in graph_nodes else []

graph_nodes = {
    'A': [('B', 2), ('E', 3)],
    'B': [('A', 2), ('C', 1), ('G', 9)],
    'C': [('B', 1)],
    'D': [('E', 6), ('G', 1)],
    'E': [('A', 3), ('D', 6)],
    'G': [('B', 9), ('D', 1)]
}

uniformCostSearchAlgo('A', 'G')
# Output:
# Optimal Path: ['A', 'E', 'D', 'G']

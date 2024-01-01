# search_Algorithms
Many problems that may be solved using which of two Depth First Search or Breadth First Search.

# Shortest Bridge 

#The purpose of solving this problem using bfs is how to connect two islands that between them is a water.
#flib islands which means make your ones zeros.
#The general steps involved in this approach are as follows: 
1) Using BFS, we mark the border cells of the first island with a specific value, such as 2, to distinguish them from the inner cells.
2) Expand the border using BFS: Starting from the border cells, we perform BFS to expand the border until we reach the second island.
3) During the BFS expansion, as soon as we encounter a cell belonging to the second island, we have found the shortest bridge. We return the number of steps taken to reach that cell.
  

# Code
from queue import Queue

class Node:
    def __init__(self, row, column):
        self.row = row
        self.column = column

def shortest_bridge(input):
    def find_island(input):
        for i in range(len(input)):
            for j in range(len(input[0])):
                if input[i][j] == 1:
                    return bfs(input, i, j)
        return None

    def bfs(input, i, j):
        input[i][j] = 2
        queue = Queue()
        queue.put(Node(i, j))
        output = Queue()

        while not queue.empty():
            node = queue.get()
            output.put(node)
            i, j = node.row, node.column
            add_first_island(input, i + 1, j, queue)
            add_first_island(input, i - 1, j, queue)
            add_first_island(input, i, j + 1, queue)
            add_first_island(input, i, j - 1, queue)

        return output

    def add_second_island(input, i, j, queue):
        if 0 <= i < len(input) and 0 <= j < len(input[0]) and input[i][j] not in [2, 3, 4]:
            input[i][j] = 3 if input[i][j] == 0 else 4
            queue.put(Node(i, j))

    def add_first_island(input, i, j, queue):
        if 0 <= i < len(input) and 0 <= j < len(input[0]) and input[i][j] == 1:
            input[i][j] = 2
            queue.put(Node(i, j))

    island = find_island(input)
    island.put(None)
    count = 0

    while not island.empty():
        node = island.get()
        if node is None and not island.empty():
            count += 1
            island.put(None)
        elif input[node.row][node.column] == 4:
            return count - 1
        else:
            i, j = node.row, node.column
            add_second_island(input, i + 1, j, island)
            add_second_island(input, i - 1, j, island)
            add_second_island(input, i, j + 1, island)
            add_second_island(input, i, j - 1, island)

    return count

# Example usage:
input_matrix = [
    [0, 1, 0],
    [0, 0, 0],
    [0, 0, 1]
]

result = shortest_bridge(input_matrix)
print("Length of the shortest bridge:", result)

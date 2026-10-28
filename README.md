<h1>ExpNo 4 : Implement A* search algorithm for a Graph</h1> 
<h3>Name: VIDHULPRABA R  </h3>
<h3>Register Number:    212225040488     </h3>
 DATE: 26/10/2026
<H3>Aim:</H3>
<p>To ImplementA * Search algorithm for a Graph using Python 3.</p>
<H3>Algorithm:</H3>

``````

from collections import defaultdict
import heapq
import networkx as nx
import matplotlib.pyplot as plt


def a_star(graph, heuristic, start, goal):
    open_set = [(heuristic[start], 0, start)]
    g = {start: 0}
    parent = {start: None}

    while open_set:
        f_score, cost_so_far, current = heapq.heappop(open_set)

        # Skip outdated entries in the priority queue
        if cost_so_far != g.get(current, float("inf")):
            continue

        if current == goal:
            path = []
            node = current

            while node is not None:
                path.append(node)
                node = parent[node]

            path.reverse()

            print("\nShortest Path:", " -> ".join(path))
            print("Total Cost:", g[goal])
            return path

        for neighbour, cost in graph[current]:
            new_cost = g[current] + cost

            if new_cost < g.get(neighbour, float("inf")):
                g[neighbour] = new_cost
                parent[neighbour] = current

                f = new_cost + heuristic[neighbour]
                heapq.heappush(
                    open_set, (f, new_cost, neighbour)
                )

    print("Path does not exist!")
    return None


# Create graph
graph = defaultdict(list)
G = nx.Graph()

n, e = map(
    int, input("Enter number of nodes and edges: ").split()
)

print("\nEnter the edges (u v cost):")

for i in range(e):
    u, v, cost = input(f"Edge {i + 1}: ").split()
    cost = int(cost)

    if cost < 0:
        raise ValueError("Edge costs must be non-negative.")

    graph[u].append((v, cost))
    graph[v].append((u, cost))
    G.add_edge(u, v, weight=cost)

# Enter heuristic values
heuristic = {}

print("\nEnter Heuristic Values:")

for i in range(n):
    node, h = input(
        f"{i + 1}. Node Heuristic: "
    ).split()

    heuristic[node] = int(h)

    # Include nodes even if they have no edges
    graph.setdefault(node, [])
    G.add_node(node)

print("\nAdjacency List:")

for node in graph:
    print(node, "->", graph[node])

print("\nHeuristic Values:")
print(heuristic)

# Draw original graph
plt.figure(figsize=(8, 6))

pos = nx.spring_layout(G, seed=20)

nx.draw_networkx_nodes(
    G, pos, node_color="skyblue", node_size=1800
)
nx.draw_networkx_labels(
    G, pos, font_size=12, font_weight="bold"
)
nx.draw_networkx_edges(G, pos, width=2)

edge_labels = nx.get_edge_attributes(G, "weight")
nx.draw_networkx_edge_labels(
    G, pos, edge_labels=edge_labels
)

plt.title("Original Graph")
plt.axis("off")
plt.tight_layout()
plt.show()

# Enter start and goal nodes
start = input("\nEnter Start Node: ")
goal = input("Enter Goal Node: ")

if start not in heuristic:
    print("Invalid Start Node!")

elif goal not in heuristic:
    print("Invalid Goal Node!")

else:
    shortest_path = a_star(
        graph, heuristic, start, goal
    )

    if shortest_path:
        # Get edges belonging to the shortest path
        path_edges = [
            (shortest_path[i], shortest_path[i + 1])
            for i in range(len(shortest_path) - 1)
        ]

        # Draw graph with shortest path highlighted
        plt.figure(figsize=(8, 6))

        nx.draw_networkx_nodes(
            G, pos, node_color="skyblue", node_size=1800
        )
        nx.draw_networkx_labels(
            G, pos, font_size=12, font_weight="bold"
        )

        # Draw all edges in gray
        nx.draw_networkx_edges(
            G, pos, edge_color="gray", width=2
        )

        # Highlight shortest path in red
        nx.draw_networkx_edges(
            G,
            pos,
            edgelist=path_edges,
            edge_color="red",
            width=4
        )

        nx.draw_networkx_edge_labels(
            G, pos, edge_labels=edge_labels
        )

        plt.title("Shortest Path Highlighted (Red)")
        plt.axis("off")
        plt.tight_layout()
        plt.show()

``````

<hr>
<h2>Sample Graph I</h2>
<hr>

<img width="1346" height="782" alt="Screenshot 2026-10-28 101238" src="https://github.com/user-attachments/assets/919c8e00-b303-4713-988d-79b46d37e1c4" />

<hr>
<h2>Sample Input</h2>
<hr>
10 14 <br>
A B 6 <br>
A F 3 <br>
B D 2 <br>
B C 3 <br>
C D 1 <br>
C E 5 <br>
D E 8 <br>
E I 5 <br>
E J 5 <br>
F G 1 <br>
G I 3 <br>
I J 3 <br>
F H 7 <br>
I H 2 <br>
A 10 <br>
B 8 <br>
C 5 <br>
D 7 <br>
E 3 <br>
F 6 <br>
G 5 <br>
H 3 <br>
I 1 <br>
J 0 <br>
<hr>
<h2>Sample Output</h2>
<hr>
Path found: ['A', 'F', 'G', 'I', 'J']


<hr>
<h2>Sample Graph II</h2>
<hr>

![image](https://github.com/natsaravanan/19AI405FUNDAMENTALSOFARTIFICIALINTELLIGENCE/assets/87870499/acbb09cb-ed39-48e5-a59b-2f8d61b978a3)


<hr>
<h2>Sample Input</h2>
<hr>
6 6 <br>
A B 2 <br>
B C 1 <br>
A E 3 <br>
B G 9 <br>
E D 6 <br>
D G 1 <br>
A 11 <br>
B 6 <br>
C 99 <br>
E 7 <br>
D 1 <br>
G 0 <br>
<hr>
<h2>Sample Output</h2>
<hr>
Path found: ['A', 'E', 'D', 'G']

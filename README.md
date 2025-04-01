class Node:
    def __init__(self, name, is_maximizer=False, value=None):
        self.name = name
        self.is_maximizer = is_maximizer
        self.value = value
        self.children = []

def alpha_beta(node, depth, alpha, beta, maximizing_player):
    if depth == 0 or not node.children:
        return node.value
    
    if maximizing_player:
        value = float('-inf')
        for child in node.children:
            value = max(value, alpha_beta(child, depth-1, alpha, beta, False))
            alpha = max(alpha, value)
            if alpha >= beta:
                print(f"Pruning at {child.name} (Beta cutoff)")
                break  
        return value
    else:
        value = float('inf')
        for child in node.children:
            value = min(value, alpha_beta(child, depth-1, alpha, beta, True))
            beta = min(beta, value)
            if beta <= alpha:
                print(f"Pruning at {child.name} (Alpha cutoff)")
                break  
        return value
A = Node('A', is_maximizer=True)
B1 = Node('B1')
A.children = [B1]
terminal_nodes = [
    ('C2', 12), ('C3', 10), ('C4', 3),
    ('C5', 5), ('C6', 8), ('C7', 10),
    ('C8', 11), ('C9', 2), ('C10', 12)
]

for name, value in terminal_nodes:
    B1.children.append(Node(name, value=value))


result = alpha_beta(A, depth=3, alpha=float('-inf'), beta=float('inf'), maximizing_player=True)
print(f"\nOptimal value at root node A: {result}")

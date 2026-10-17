                              PRACTICAL NO :  1
Aim: Write programs for the following: a. Implement depth first search algorithm. 
graph1 = {
    'A': set(['B', 'C']),
    'B': set(['D', 'E']),
    'C': set(['A', 'F']),
    'D': set(['B']),
    'E': set(['B', 'F']),
    'F': set(['E', 'C'])
}

def dfs(graph, node, visited):
    if node not in visited:
        visited.append(node)
        for n in graph[node]:
            dfs(graph, n, visited)
    return visited

visited = dfs(graph1, 'A', [])
print(visited)


         PRACTICAL NO. 1 (b)
Aim: Implement Breadth First Search (BFS) algorithm.
graph = {
    'A': set(['B', 'C']),
    'B': set(['D', 'E', 'A']),
    'C': set(['A', 'F']),
    'D': set(['B']),
    'E': set(['B', 'F']),
    'F': set(['E', 'C'])
}

def bfs(start):
    queue = [start]
    levels = {}
    levels[start] = 0
    visited = set(start)

    while queue:
        node = queue.pop(0)
        neighbors = graph[node]

        for neighbor in neighbors:
            if neighbor not in visited:
                queue.append(neighbor)
                visited.add(neighbor)
                levels[neighbor] = levels[node] + 1

    print(levels)
    return visited

print(str(bfs('A')))




                              PRACTICAL NO. 2
                              
Aim: Write a program to implement the A* algorithm.
Code:


Graph_nodes = {
    'A': [('B', 6), ('F', 3)],
    'B': [('C', 3), ('D', 2)],
    'C': [('D', 1), ('E', 5)],
    'D': [('C', 1), ('E', 8)],
    'E': [('I', 5), ('J', 5)],
    'F': [('G', 1), ('H', 7)],
    'G': [('I', 3)],
    'H': [('I', 2)],
    'I': [('E', 5), ('J', 3)]
}


def get_neighbors(v):
    if v in Graph_nodes:
        return Graph_nodes[v]
    else:
        return None


def h(n):
    H_dist = {
        'A': 10,
        'B': 8,
        'C': 5,
        'D': 7,
        'E': 3,
        'F': 6,
        'G': 5,
        'H': 3,
        'I': 1,
        'J': 0
    }
    return H_dist[n]


def aStarAlgo(start_node, stop_node):
    open_set = set([start_node])
    closed_set = set()
    g = {}
    parents = {}

    g[start_node] = 0
    parents[start_node] = start_node

    while len(open_set) > 0:
        n = None

        for v in open_set:
            if n is None or g[v] + h(v) < g[n] + h(n):
                n = v

        if n == stop_node or Graph_nodes[n] is None:
            pass
        else:
            for (m, weight) in get_neighbors(n):
                if m not in open_set and m not in closed_set:
                    open_set.add(m)
                    parents[m] = n
                    g[m] = g[n] + weight

        if n == stop_node:
            path = []

            while parents[n] != n:
                path.append(n)
                n = parents[n]

            path.append(start_node)
            # path.reverse()

            print('Path found: {}'.format(path))
            return path

        open_set.remove(n)
        closed_set.add(n)

    print('Path does not exist!')
    return None


aStarAlgo('A', 'J')



                      
                      PRACTICAL NO. 3
                      
Aim: Write a program to implement the Hill Climbing problem.


Succlist = {
    'A': [['B', 3], ['C', 2]],
    'B': [['D', 2], ['E', 3]],
    'C': [['F', 1], ['G', 4]],
    'D': [['H', 1], ['I', 99]],
    'F': [['J', 1]],
    'G': [['K', 99], ['L', 3]]
}

start = 'A'
closed = list()
success = True
failure = False


def movegen(N):
    newList = list()
    if N in Succlist.keys():
        newList = Succlist[N]
        return newList


def sort(L):
    L.sort(key=lambda x: x[1])
    return L


def heu(Node):
    return Node[1]


def append(L1, L2):
    newList = list(L1) + list(L2)
    return newList


def hill_climbing(start):
    global closed

    N = start
    child = movegen(N)
    sort(child)

    N = [start, 5]

    print("\nStart =", N)
    print("Sorted child list:", child)

    newNode = child[0]
    closed = [N]

    while heu(newNode) < heu(N) and len(child) != 0:
        print("\n_______________________________________")

        N = newNode
        print("N:", N)

        closed = append(closed, [N])
        child = movegen(N[0])
        sort(child)

        print("Sorted child list:", child)
        print("Closed:", closed)

        newNode = child[0]


hill_climbing(start)



                      
                      PRACTICAL NO. 4
                      
Aim: Simulate Tic-Tac-Toe game using Min-Max algorithm.
Code:

import os
import time

board = ['', '', '', '', '', '', '', '', '', '']
player = 1

Win = 1
Draw = -1
Running = 0
stop = 1

Game = Running
Mark = "X"


def DrawBoard():
    print(" %s  | %s  | %s " % (board[1], board[2], board[3]))
    print("____|____|____")
    print(" %s  | %s  | %s " % (board[4], board[5], board[6]))
    print("____|____|____")
    print(" %s  | %s  | %s " % (board[7], board[8], board[9]))
    print("    |    |    ")


def CheckPosition(x):
    if board[x] == '':
        return True
    else:
        return False


def CheckWin():
    global Game

    # Horizontal
    if board[1] == board[2] == board[3] != '':
        Game = Win
    elif board[4] == board[5] == board[6] != '':
        Game = Win
    elif board[7] == board[8] == board[9] != '':
        Game = Win

    # Vertical
    elif board[1] == board[4] == board[7] != '':
        Game = Win
    elif board[2] == board[5] == board[8] != '':
        Game = Win
    elif board[3] == board[6] == board[9] != '':
        Game = Win

    # Diagonal
    elif board[1] == board[5] == board[9] != '':
        Game = Win
    elif board[3] == board[5] == board[7] != '':
        Game = Win

    # Draw
    elif all(board[i] != '' for i in range(1, 10)):
        Game = Draw
    else:
        Game = Running


print(":::::::::::: TIC TAC TOE ::::::::::::")
print("Player 1 [X] ----- Player 2 [O]")
print("Please wait...")
time.sleep(1)

while Game == Running:
    os.system("cls" if os.name == "nt" else "clear")
    DrawBoard()

    if player % 2 != 0:
        print("Player 1's Chance")
        Mark = "X"
    else:
        print("Player 2's Chance")
        Mark = "O"

    choice = int(input("Enter position [1-9]: "))

    if choice < 1 or choice > 9:
        print("Invalid position! Try again.")
        time.sleep(1)
        continue

    if CheckPosition(choice):
        board[choice] = Mark
        player += 1
        CheckWin()
    else:
        print("Position already occupied! Try again.")
        time.sleep(1)

os.system("cls" if os.name == "nt" else "clear")
DrawBoard()

if Game == Draw:
    print("Game Draw")
elif Game == Win:
    player -= 1

    if player % 2 != 0:
        print("Player 1 Won!")
    else:
        print("Player 2 Won!")




                                 PRACTICAL NO. 5
                                 
Aim: Write a program to design an application to simulate the Number Puzzle problem.
Code:

def print_in_format(matrix):
    for i in range(9):
        if i % 3 == 0 and i > 0:
            print("")
        print(str(matrix[i]) + " ", end="")


def count(s):
    c = 0
    ideal = [
        1, 2, 3,
        4, 5, 6,
        7, 8, 0
    ]

    for i in range(9):
        if s[i] != 0 and s[i] != ideal[i]:
            c += 1

    return c


def move(ar, p, st):
    rh = 9999
    store_st = st.copy()

    for i in range(len(ar)):
        dupl_st = st.copy()

        tmp = dupl_st[p]
        dupl_st[p] = dupl_st[ar[i]]
        dupl_st[ar[i]] = tmp

        trh = count(dupl_st)

        print("Value of p:", p, "Value of i:", i)
        print("Before checking condition trh =", trh, "rh =", rh)

        if trh < rh:
            rh = trh
            store_st = dupl_st.copy()

        print("After checking condition store_st:",
              store_st, "trh =", trh, "rh =", rh)

    return store_st, rh


state = [
    1, 2, 3,
    0, 5, 6,
    4, 7, 8
]

h = count(state)
Level = 1

print("\n---------Level", Level, "------------")
print_in_format(state)
print("\nHeuristic value (Misplaced):", h)

while h > 0:
    pos = int(state.index(0))
    print("\npos:", pos)

    Level += 1

    if pos == 0:
        arr = [1, 3]

    elif pos == 1:
        arr = [0, 2, 4]

    elif pos == 2:
        arr = [1, 5]

    elif pos == 3:
        arr = [0, 4, 6]

    elif pos == 4:
        arr = [1, 3, 5, 7]

    elif pos == 5:
        arr = [2, 4, 8]

    elif pos == 6:
        arr = [3, 7]

    elif pos == 7:
        arr = [4, 6, 8]

    elif pos == 8:
        arr = [5, 7]

    state, h = move(arr, pos, state)

    print("\n=============Level", Level, "===============")
    print_in_format(state)
    print("\nHeuristic Value (Misplaced):", h)




                         
                           PRACTICAL NO. 6
                           
Aim: Write a program to solve a Constraint Satisfaction Problem (CSP).
Installation (CMD) : pip install simpleai

code :
from simpleai.search import CspProblem, backtrack

variables = ('A', 'B', 'C', 'D')

domains = {
    'A': ['red', 'green', 'blue'],
    'B': ['red', 'green', 'blue'],
    'C': ['red', 'green', 'blue'],
    'D': ['red', 'green', 'blue']
}


def different_color(variables, values):
    return values[0] != values[1]


constraints = [
    (('A', 'B'), different_color),
    (('A', 'C'), different_color),
    (('B', 'D'), different_color),
    (('B', 'C'), different_color),
    (('C', 'D'), different_color)
]

problem = CspProblem(variables, domains, constraints)

solution = backtrack(problem)

print("Solution:")
print(solution)




                             PRACTICAL NO. 7
                             
Aim: Write a program to simulate the 4-Queen / N-Queen problem.
Code:

global N
N = 4


def printSolution(board):
    print("The Solution is:")

    for i in range(N):
        for j in range(N):
            print(board[i][j], end=' ')
        print()


def isQSafe(board, row, col):

    # Check row on the left side
    for i in range(col):
        if board[row][i] == 1:
            return False

    # Check upper diagonal
    for i, j in zip(
        range(row, -1, -1),
        range(col, -1, -1)
    ):
        if board[i][j] == 1:
            return False

    # Check lower diagonal
    for i, j in zip(
        range(row, N, 1),
        range(col, -1, -1)
    ):
        if board[i][j] == 1:
            return False

    return True


def solveNQUtil(board, col):

    if col >= N:
        return True

    for i in range(N):
        if isQSafe(board, i, col):

            board[i][col] = 1

            if solveNQUtil(board, col + 1) == True:
                return True

            board[i][col] = 0

    return False


def solveNQ():

    board = [
        [0, 0, 0, 0],
        [0, 0, 0, 0],
        [0, 0, 0, 0],
        [0, 0, 0, 0]
    ]

    if solveNQUtil(board, 0) == False:
        print("Solution does not exist")
        return False

    printSolution(board)
    return True


solveNQ()



                         PRACTICAL NO. 8
                         
Aim: Write a program to solve the Water Jug Problem.
Code:

j1 = int(input("Capacity of jug 1: "))
j2 = int(input("Capacity of jug 2: "))
g = int(input("Amount of water to be measured: "))


def apply_rule(ch, x, y):

    # Rule 1: Fill jug 1
    if ch == 1:
        if x < j1:
            return j1, y
        else:
            print("Rule cannot be applied")
            return x, y

    # Rule 2: Fill jug 2
    elif ch == 2:
        if y < j2:
            return x, j2
        else:
            print("Rule cannot be applied")
            return x, y

    # Rule 3: Transfer all water from jug 1 to jug 2
    elif ch == 3:
        if x > 0 and x + y <= j2:
            return 0, x + y
        else:
            print("Rule cannot be applied")
            return x, y

    # Rule 4: Transfer all water from jug 2 to jug 1
    elif ch == 4:
        if y > 0 and x + y <= j1:
            return x + y, 0
        else:
            print("Rule cannot be applied")
            return x, y

    # Rule 5: Transfer water from jug 1 until jug 2 is full
    elif ch == 5:
        if x > 0 and x + y >= j2:
            return j2, x - (j2 - y)
        else:
            print("Rule cannot be applied")
            return x, y

    # Rule 6: Transfer water from jug 2 until jug 1 is full
    elif ch == 6:
        if y > 0 and x + y >= j1:
            return j1, y - (j1 - x)
        else:
            print("Rule cannot be applied")
            return x, y

    # Rule 7: Empty jug 1
    elif ch == 7:
        if x > 0:
            return 0, y
        else:
            print("Rule cannot be applied")
            return x, y

    # Rule 8: Empty jug 2
    elif ch == 8:
        if y > 0:
            return x, 0
        else:
            print("Rule cannot be applied")
            return x, y

    else:
        print("INVALID CHOICE")
        return x, y


x = y = 0

while True:

    if x == g or y == g:
        print("GOAL ACHIEVED!!")
        break

    print("\n================ RULES ================")
    print("Rule 1: Fill jug 1")
    print("Rule 2: Fill jug 2")
    print("Rule 3: Transfer all water from jug 1 to jug 2")
    print("Rule 4: Transfer all water from jug 2 to jug 1")
    print("Rule 5: Transfer water from jug 1 until jug 2 is full")
    print("Rule 6: Transfer water from jug 2 until jug 1 is full")
    print("Rule 7: Empty jug 1")
    print("Rule 8: Empty jug 2")

    ch = int(input("Enter rule to apply: "))

    x, y = apply_rule(ch, x, y)

    print("\n================ STATUS ================")
    print("CURRENT STATE:", x, y)


      
       
              PRACTICAL NO. 9 (a)
              
Aim: Derive the expressions based on Associative Law.
Code:
a, b, c = 5, 3, 2

lhs_add = (a + b) + c
rhs_add = a + (b + c)

lhs_mul = (a * b) * c
rhs_mul = a * (b * c)

print("Associative Law of Addition:")
print(f"(a+b)+c = {lhs_add}")
print(f"a+(b+c) = {rhs_add}")
print("Equal:", lhs_add == rhs_add)

print("\nAssociative Law of Multiplication:")
print(f"(a*b)*c = {lhs_mul}")
print(f"a*(b*c) = {rhs_mul}")
print("Equal:", lhs_mul == rhs_mul)



                 PRACTICAL NO. 9 (b)
                 
Aim: Derive the expressions based on Distributive Law.
Code:
a, b, c = 5, 3, 2

lhs = a * (b + c)
rhs = (a * c) + (a * b)

print("Distributive Law:")
print("LHS:", lhs)
print("RHS:", rhs)
print("Equal:", lhs == rhs)




                        PRACTICAL NO. 10

Aim: Write a program to solve the Tower of Hanoi problem.
Code:

def movetower(height, start, aux, end):
    if height >= 1:
        movetower(height - 1, start, end, aux)

        print("Moving disk from", start, "to", end)

        movetower(height - 1, aux, start, end)

movetower(3, "A", "B", "C")




                        PRACTICAL NO. 11
                        
Aim: Write a program to shuffle a deck of cards.
Code:

import random
import itertools

deck = list(
    itertools.product(
        range(1, 14),
        ['Spade', 'Club', 'Heart', 'Diamond']
    )
)

random.shuffle(deck)

print(deck)

for i in range(5):
    print(deck[i][0], 'of', deck[i][1])



              
              
                     PRACTICAL NO. 12
                     
Aim: Write a program to derive predicates. For example, Sachin is a batsman, and a batsman is a cricketer.
Statements:
- Sachin is a cricketer.
- Dhoni is a wicketkeeper.
- Ronaldo is a footballer, and a footballer is not a cricketer.
Steps:
1. Download and install GNU Prolog from Chrome.
2. Open Notepad and write the facts and rules.
3. Save the file as cricketer.pl.
4. Open the GNU Prolog console.
5. Use the consult command to load the file.
6. Run queries and interpret the output.

Code (cricketer.pl):
batsman(sachin).
wicketkeeper(dhoni).
footballer(ronaldo).

cricketer(X) :- batsman(X).
cricketer(X) :- wicketkeeper(X).

not_cricketer(X) :- footballer(X).


Output console Queries:
consult('cricketer.pl').
cricketer(sachin).
cricketer(ronaldo).
wicketkeeper(dhoni).
footballer(ronaldo).
wicketkeeper(sachin).
not_cricketer(ronaldo).






                      PRACTICAL NO. 13
                      
Aim: Write a program containing three predicates: male, female, and parent. Make rules for family relations such as father, mother, grandfather, grandmother, brother, sister, uncle, aunt, nephew, niece, and cousin.

Code (family.pl)

female(pam).
female(pat).
female(liz).
female(ann).
female(jim).

male(tom).
male(peter).

parent(pam,bob).
parent(tom,bob).
parent(tom,liz).
parent(bob,ann).
parent(pat,jim).
parent(pat,ann).
parent(bob,peter).
parent(peter,jim).

mother(X,Y):-parent(X,Y),female(X).

father(X,Y):-parent(X,Y),male(X).

sister(X,Y):-parent(Z,X),parent(Z,Y),female(X),X\==Y.

brother(X,Y):-parent(Z,X),parent(Z,Y),male(X),X\==Y.

grandparent(X,Y):-parent(X,Z),parent(Z,Y).

grandmother(X,Y):-mother(X,Y),parent(Y,Z).

grandfather(X,Y):-father(X,Y),parent(Y,Z).

wife(X,Y):-parent(X,Z),parent(Y,Z),female(X),male(Y).

uncle(X,Z):-brother(X,Y),parent(Y,Z).





                                   PRACTICAL NO. 14
                                   
Aim: Write a program to build a simple rule-based expert system using Python (e.g., a medical diagnosis system).
Code:

def diagnosis(symptoms):
    rules = {
        ("fever", "cough"): "Flu",
        ("headache", "fever"): "Dengue",
        ("cough", "shortness of breath"): "COVID-19"
    }

    for key, disease in rules.items():
        if all(symptom in symptoms for symptom in key):
            return disease

    return "Unknown"


print("You have: ", diagnosis(["fever", "cough"]))




                             PRACTICAL NO. 15
                             
Aim: Build a simple rule-based expert system using Python to suggest a career path.
Code:

def careerpath(subjects):
    career_rules = {
        ("physics", "chemistry", "maths"): [
            "Engineering (Mechanical, Civil, Electrical, Computer Science, etc.)",
            "Architecture",
            "Pure Sciences (Physics, Chemistry, Mathematics)",
            "Research and Development",
        ],
        ("physics", "chemistry", "biology"): [
            "Medicine (MBBS, BDS)",
            "Pharmacy",
            "Biotechnology",
            "Microbiology",
            "Pure Sciences (Physics, Chemistry, Biology)"
        ],
        ("accountancy", "business studies", "economics"): [
            "Chartered Accountancy (CA)",
            "Company Secretary (CS)",
            "Business Management",
            "Economics",
            "Cost and Management Accounting",
            "Banking"
        ],
        ("history", "geography", "political science"): [
            "Civil Services (IAS, IPS, IFS)",
            "Law",
            "Journalism and Mass Communication",
            "Teaching",
            "Public Administration"
        ],
        ("computer science", "maths", "logical reasoning"): [
            "Software Engineering",
            "Information Technology"
        ]
    }

    for key, careers in career_rules.items():
        if all(subject in subjects for subject in key):
            return careers
    return "Unknown"


s1 = input("Enter subject 1: ")
s2 = input("Enter subject 2: ")
s3 = input("Enter subject 3: ")

print("Preferred career options:", careerpath([s1, s2, s3]))



class Node:
    def __init__(self, value):
        self.value = value
        self.next = None

class Stack:
    def __init__(self):
        self.top = None
        self._size = 0
    
    def push(self, element):
        new_node = Node(element)
        new_node.next = self.top
        self.top = new_node
        self._size += 1
    
    def pop(self):
        if self.is_empty():
            raise IndexError("Pop from empty stack")
        value = self.top.value
        self.top = self.top.next
        self._size -= 1
        return value
    
    def peek(self):
        if self.is_empty():
            return None
        return self.top.value
    
    def is_empty(self):
        return self.top is None
    
    def size(self):
        return self._size

# Validación de paréntesis balanceados
def is_balanced(expression):
    stack = Stack()
    pairs = {')': '(', ']': '[', '}': '{'}
    
    for char in expression:
        if char in "({[":
            stack.push(char)
        elif char in ")}]":
            if stack.is_empty() or stack.pop() != pairs[char]:
                return False
    
    return stack.is_empty()

# Conversión de notación infija a postfija
def infix_to_postfix(expression):
    precedence = {'+': 1, '-': 1, '*': 2, '/': 2, '^': 3}
    stack = Stack()
    output = []
    tokens = expression.split()
    
    for token in tokens:
        if token.isalnum():  # Operando
            output.append(token)
        elif token in precedence:  # Operador
            while (not stack.is_empty() and stack.peek() in precedence and
                   precedence[stack.peek()] >= precedence[token]):
                output.append(stack.pop())
            stack.push(token)
        elif token == '(':
            stack.push(token)
        elif token == ')':
            while not stack.is_empty() and stack.peek() != '(':
                output.append(stack.pop())
            stack.pop()
    
    while not stack.is_empty():
        output.append(stack.pop())
    
    return ' '.join(output)

# Ejemplos de uso
expresion1 = "( 3 + 2 ) * ( 8 / 4 )"
expresion2 = "(( 3 + 2 ) * ( 8 / 4 )"
print("Expresión balanceada:", is_balanced(expresion1))  # True
print("Expresión balanceada:", is_balanced(expresion2))  # False

expresion3 = "3 + 5 * ( 2 - 8 )"
print("Notación postfija:", infix_to_postfix(expresion3))  # "3 5 2 8 - * +"

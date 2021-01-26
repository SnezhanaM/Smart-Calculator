from collections import deque


class SmartCalculator:
    variables = {}

    def __init__(self):
        self.variables = {}

    def infix_to_postfix(self, string: str):
        ops = {"+": 0, "-": 0, "*": 1, "/": 1, "^": 2, "(": -1, ")": -1}

        if string == "":
            return None

        while string.count("--") != 0:
            string = string.replace("--", "+")
        while string.count("++") != 0:
            string = string.replace("++", "+")
        string = string.replace("+-", "-")

        string = string.replace("+", " + ")
        string = string.replace("-", " - ")
        string = string.replace("*", " * ")
        string = string.replace("/", " / ")
        string = string.replace("^", " ^ ")
        string = string.replace("(", " ( ")
        string = string.replace(")", " ) ")

        infix = string.split()
        for i in range(len(infix) - 1):
            if infix[i] not in ops.keys() and infix[i + 1] not in ops.keys():
                return "Invalid expression"

        stack = deque()
        postfix = []
        for i in infix:
            if i in self.variables:
                i = self.variables[i]
            try:
                postfix.append(float(i))
            except ValueError:
                try:
                    if (i not in ops.keys()) and (i not in "()"):
                        return "Unknown variable"
                    else:
                        if len(stack) == 0 or i == "(":
                            stack.append(i)
                        elif i == ")":
                            top = stack.pop()
                            while top != "(":
                                postfix.append(top)
                                top = stack.pop()
                        else:
                            top = stack.pop()
                            if top == "(" or ops[i] > ops[top]:
                                stack.append(top)
                                stack.append(i)
                            else:
                                while top != "(" and ops[i] <= ops[top]:
                                    postfix.append(top)
                                    if len(stack) == 0:
                                        break
                                    else:
                                        top = stack.pop()
                                else:
                                    stack.append(top)
                                stack.append(i)
                except (KeyError, IndexError):
                    return "Invalid expression"
        while len(stack) != 0:
            postfix.append(stack.pop())
        if "(" in postfix:
            return "Invalid expression"

        return postfix

    def operation(self, second, first, operand):
        if operand == "+":
            return first + second
        elif operand == "-":
            return first - second
        elif operand == "*":
            return first * second
        elif operand == "/":
            return float(first / second)
        elif operand == "^":
            return first ** second

    def eval_postfix(self, postfix: list):
        if not postfix:
            return None
        if postfix in ("Invalid expression", "Unknown variable"):
            return postfix
        ops = ["+", "-", "*", "/", "^"]
        stack = deque()

        try:
            for i in postfix:
                if i not in ops:
                    stack.append(i)
                else:
                    result = self.operation(stack.pop(), stack.pop(), i)
                    stack.append(result)
        except IndexError:
            return "Invalid expression"
        return stack.pop()

    def calc_vars(self, string: str):
        for key in self.variables.keys():
            string = string.replace(key, self.variables[key])
        return string

    def assign_var(self, variable: str, value: str):
        variable = variable.strip()
        if not variable.isalpha():
            return "Invalid identifier"
        assignment = self.eval_postfix(self.infix_to_postfix(value))
        if assignment in ("Invalid expression", "Unknown variable"):
            return "Invalid assignment"
        self.variables[variable] = str(assignment)

    def main(self):
        while True:
            user_input = input()
            if user_input.startswith("/"):
                if user_input == '/exit':
                    print('Bye!')
                    break
                elif user_input == "/help":
                    print("""Smart calculator can calculate +, -, *, /
and exponentiation, supports variables.
To get started, type the expression for the calculation.""")
                else:
                    print("Unknown command")
            else:
                result = None
                if "=" in user_input:
                    assignment = user_input.split("=")
                    try:
                        result = self.assign_var(*assignment)
                    except TypeError:
                        print("Invalid assignment")
                else:
                    infix = self.calc_vars(user_input)
                    result = self.eval_postfix(self.infix_to_postfix(infix))
                if result is not None:
                    print(result)


calc = SmartCalculator()
calc.main()


### Find two devisor

```.py
def findTwoDivisor(number: int):
    x = 0
    y = 2
    for i in range(number // 2):
        if number % y == 0:
            x += 1
        y += 2
    return x
```

```.py
class quiz_29:
        def __init__(self, number):
                self.number = number

        def solve():
                solution = 0
                y = 2
                for i in range(number // 2):
                    if number % y == 0:
                        solution += 1
                    y += 2
                return solution


answer = quiz_29(number=8)
print(answer.solve())
answer = quiz_29(number=49)
print(answer.solve())
answer = quiz_29(number=1000)
print(answer.solve())
```

![]()

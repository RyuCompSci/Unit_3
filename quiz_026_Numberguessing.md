### Number guessing game

```.py
import random

answer = random.randint(0,100)
x = int(input("ENter your guess: "))
y = 0
while x != answer:
    y = y + 1
    if x > answer:
        print(f"lower, tries:{y}")
    else:
        print(f"higher, tries:{y}")
    x = int(input("Enter your guess: "))
print("Correct")
print(f"Your tries are {y} times total")
```

![](image.quiz_026.png)

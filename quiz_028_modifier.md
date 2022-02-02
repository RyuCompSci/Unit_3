### modifier

```.py
def modifier(N, word):
    for i in range(N):
        f = open(f"{i+1}-sales.txt", "w")
        f.write(f"{word} {i+1}")
        f.close()
```

![](image.quiz_028.png)

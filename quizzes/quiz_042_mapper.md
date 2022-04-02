### mapper

```.py
def mapper(n: int):
    out = []
    for j in range(n):
        x = []
        for i in range(1, j+2):
            x.append(i)
        out.append(x)
    return out
```

![](image_quiz_042.png)

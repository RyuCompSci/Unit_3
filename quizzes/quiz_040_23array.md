### fix23

```.py
def fix23(lst):
    for i in range(len(lst)-1):
        if lst[i] == 2 and lst[i+1] == 3:
            lst[i+1] = 0
    return lst
```

![](image_quiz_40.png)

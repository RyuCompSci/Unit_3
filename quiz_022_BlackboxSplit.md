### BlackboxSplit

```.py
def BlackBox(input:str):
    x = input.split( )
    out = ""
    for i in range(len(x)):
        if len(x[i]) >= 3:
            out += f"{x[i][0]}{str(len(x[i])-2)}{x[i][len(x[i])-1]} "
        else:
            out += f"{x[i]} "
    return out
```

![]()

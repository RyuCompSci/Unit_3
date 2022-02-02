### BracketEmoji

```.py
def bracketEmoji(inp1, inp2):
    length = len(inp2)
    rev = ""
    x = length - 1
    for i in range(length):
        rev += inp2[x]
        x -= 1
    answer = inp2+inp1+rev
    return answer
```

![]()

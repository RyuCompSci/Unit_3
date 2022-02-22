### Matchstick

```.py
class Matchstick:
    def __init__(self, l: int, s: int):
        self.l = l
        self.s = s

    def required_matchsticks(self):
        num = self.l / self.s * 100 / 5
        return num

    def __add__(self, other):
        self.l = self.l + other.l
        self.s = self.s + other.s
        num = self.l / self.s * 100 / 5
        return num
```

![]()

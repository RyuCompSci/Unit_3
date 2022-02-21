### Count Letter

```.py
class Countletter:

    def __init__(self, text: str, letter: str):
        self.text = text
        self.letter = letter

    def NumberOfLetters(self):
        x = 0
        for i in range(len(self.text)):
            if self.letter == self.text[i]:
                x += 1
        return x


class inheritance(Countletter):
    """This is the child class of Countletter"""
    pass


countLetter = Countletter(text="The world is vase, vast", letter="a")
print(countLetter.NumberOfLetters())
Inherit = inheritance(text="The world is vase, vast", letter="a")
print(Inherit.NumberOfLetters())
```

![]()

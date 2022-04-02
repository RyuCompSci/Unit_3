### Scores

#### 1.State the four properties of the OOP programming paradigm
####   - Encapsulation    : the idea of bundling data and methods that work on that data within one unit, like a class in Java
####   - Inheritance      : the procedure in which one class inherits the attributes and methods of another class
####   - Data abstraction : the reduction of a particular body of data to a simplified representation of the whole
####   - Polymorphism.    : the concept that you can access objects of different types through the same interface


#### 2.Identify the components of a UML diagram for Classes.
####   - Top section: Name of class
####   - Middle section: Attributes (valuable)
####   - Bottom section: Methods    (function)

```.py
class Scores:
    def __init__(self, results: list):
        self.results = results

    def get_average(self):
        """ finds the average of all the values"""
        average = 0
        for i in range(len(self.results)):
            average += self.results[i]
        average /= len(self.results)
        return average
```

![]()

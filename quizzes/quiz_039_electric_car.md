### electric car

```.py
import sqlite3

class Electric_car_database_handler:
    def __init__(self):
        self.connection = sqlite3.connect("quiz_39.db")
        self.cursor = self.connection.cursor()

    def create_table(self):
        # create
        self.cursor.execute("""
        CREATE TABLE  Electrical_cars (
            model  VARCHAR(100),
            brand VARCHAR(100) not null unique,
            maker VARCHAR(255) not null,
            battery_size INTEGER
            id INTEGER primary key
        );
        """)
        self.connection.commit()

    def add_data(self):
        """ add values into the table """
        self.cursor.execute("""
        INSERT INTO Electrical_cars (model, brand, maker, battery_size) values ("Taycan Turbo", "Taycan", "Prosche", 933);""")
        self.connection.commit()


Quiz_039 = Electric_car_database_handler()
Quiz_039.create_table()
Quiz_039.add_data()
```

![]()

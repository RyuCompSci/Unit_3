### electric car

```.py
CREATE TABLE  Electrical_cars (
    model  VARCHAR(100),
    brand VARCHAR(100) not null unique,
    maker VARCHAR(255) not null,
    battery_size INTEGER
    id INTEGER primary key
);
INSERT INTO Electrical_cars (model, brand, maker, battery_size) values ("Taycan Turbo", "Taycan", "Prosche", 933);
SELECT * from Electrical_cars
```

![](image_quiz_039.png)

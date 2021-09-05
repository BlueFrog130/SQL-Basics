# SQL Basics

Showing basic SQL commands and how to query & filter data.

[Download the people data](https://github.com/BlueFrog130/SQL-Basics/raw/main/data.csv)

Creating the table
```SQL
USE tutorial;

CREATE TABLE People (
    id INT NOT NULL AUTO_INCREMENT,
    `first` VARCHAR(100) NOT NULL,
    middle CHAR(1) NOT NULL,
    `last` VARCHAR(100) NOT NULL,
    gender VARCHAR(6) NOT NULL,
    state CHAR(2) NOT NULL,
    birthday DATETIME NOT NULL,
    PRIMARY KEY ( id )
);
```

Selecting everything from the people table
```SQL
USE tutorial;

SELECT * FROM people;
```

Inserting a single value
```SQL
USE tutorial;

INSERT INTO people (`first`, middle, `last`, gender, state, birthday) VALUES 
('Adam', 'l', 'Grady', 'male', 'SD', '1998-08-14');
```

Inserting from a csv
```SQL
USE tutorial;

LOAD DATA INFILE 'C:/Users/<USER>/Downloads/data.csv'
    INTO TABLE People
    FIELDS TERMINATED BY ','
    LINES TERMINATED BY '\n'
    IGNORE 1 ROWS
    (@`first`, @middle, @`last`, @gender, @state, @birthday)
    SET `first`=@`first`, middle=@middle, `last`=@`last`, gender=@gender, state=@state, birthday=@birthday;
```

Altering the table
```SQL
USE tutorial;

ALTER TABLE People ADD COLUMN FullName TEXT GENERATED ALWAYS AS (CONCAT(`first`, ' ', `last`));
```

Some more `SELECT` statements
```SQL
USE tutorial;

# Counts everything from the people table
SELECT COUNT(*) FROM People;

# Gets people's id, first name, and state where their state is from SD. Then limits to top 100 results.
SELECT id, `first`, state FROM people WHERE state = 'SD' LIMIT 100;

# Selects a persons full name and age
SELECT FullName, TIMESTAMPDIFF(YEAR, birthday, CURDATE()) AS Age FROM People LIMIT 100;

# Groups people by gender, then computes the average age and counts them
SELECT
    gender AS Gender,
    AVG(TIMESTAMPDIFF(YEAR, birthday, CURDATE())) AS AvgAge,
    COUNT(*) AS Amount
FROM People GROUP BY gender;
```

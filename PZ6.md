## В самом конце сплошняком есть то, как все должно выглядеть в конце + вот [САЙТ](https://onecompiler.com/postgresql/42xet4k3q) где можно выполнять

#### Запросы

1. (8) Напишите запрос, возвращающий имена и фамилии всех студентов с фамилией Воробьев.

```sql
SELECT * FROM STUDENT
WHERE SURNAME = 'Воробьев';
```

2. (10) Напишите запрос на вывод находящихся в таблице EXAM_MARKS номеров предметов обучения, экзамены по которым сдавались между 2024-12-03 и 2024-12-26.

```sql
-- для выполнения задания
SELECT SUBJ_ID
FROM EXAM_MARKS
WHERE EXAM_DATE BETWEEN '2024-12-03' AND '2024-12-26';

-- чтобы посмотреть название предмета с полученным SUBJ_ID
select * from subject where subj_id = 1

-- ниже не обязательно, просто эксперимент, объединение двух запросов в один
SELECT *
FROM SUBJECT
WHERE SUBJ_ID = (
    SELECT SUBJ_ID
    FROM EXAM_MARKS
    WHERE EXAM_DATE BETWEEN '2024-12-03' AND '2024-12-26'
);
```

#### В случае, если нужно подстроиться под задание, можно поменять значения некоторых ячеек

```sql
UPDATE EXAM_MARKS
SET <Что поменять> = 'на какое значение'
WHERE <условие>;
```
Пример:

```sql
UPDATE EXAM_MARKS
SET exam_date = '2024-12-14'
WHERE exam_id = 1;

UPDATE EXAM_MARKS
SET exam_date = '2024-12-27'
WHERE exam_id = 3;
```

<hr>

### Правильный порядок создания таблиц
1. SUBJECT and UNIVERSITY
```sql
CREATE TABLE SUBJECT (
    SUBJ_ID SERIAL PRIMARY KEY,
    SUBJ_NAME VARCHAR(100) NOT NULL,
    HOUR INTEGER,
    SEMESTER INTEGER
);

CREATE TABLE UNIVERSITY (
    UNIV_ID SERIAL PRIMARY KEY,
    UNIV_NAME VARCHAR(100) NOT NULL,
    RATING NUMERIC(3, 2),
    CITY VARCHAR(50)
);
```
2. STUDENT
```sql
CREATE TABLE STUDENT (
    STUDENT_ID SERIAL PRIMARY KEY,
    SURNAME VARCHAR(50) NOT NULL,
    NAME VARCHAR(50) NOT NULL,
    STIPEND NUMERIC(10, 2),
    KURS INTEGER,
    CITY VARCHAR(50),
    BIRTHDAY DATE,
    UNIV_ID INTEGER,
    CONSTRAINT fk_univ_id FOREIGN KEY (UNIV_ID) REFERENCES UNIVERSITY(UNIV_ID)
);
```
3. EXAM_MARKS
```sql
CREATE TABLE EXAM_MARKS (
    EXAM_ID SERIAL PRIMARY KEY,
    SUBJ_ID INTEGER,
    STUDENT_ID INTEGER,
    MARK NUMERIC(3, 1),
    EXAM_DATE DATE,
    CONSTRAINT fk_student FOREIGN KEY (STUDENT_ID) REFERENCES STUDENT(STUDENT_ID) ON DELETE CASCADE,
    CONSTRAINT fk_subject FOREIGN KEY (SUBJ_ID) REFERENCES SUBJECT(SUBJ_ID) ON DELETE CASCADE
);
```
4. LECTURER
```sql
CREATE TABLE LECTURER (
    LECTURER_ID SERIAL PRIMARY KEY,
    SURNAME VARCHAR(50) NOT NULL,
    NAME VARCHAR(50) NOT NULL,
    CITY VARCHAR(50),
    UNIV_ID INTEGER,
    CONSTRAINT fk_univ_id FOREIGN KEY (UNIV_ID) REFERENCES UNIVERSITY(UNIV_ID)
);
```
5. SUBJ_LECT
```sql
CREATE TABLE SUBJ_LECT (
    LECTURER_ID INTEGER, 
    SUBJ_ID INTEGER,
    PRIMARY KEY (LECTURER_ID, SUBJ_ID),
    CONSTRAINT fk_lecturer_id FOREIGN KEY (LECTURER_ID) REFERENCES LECTURER(LECTURER_ID) ON DELETE CASCADE,
    CONSTRAINT fk_subject FOREIGN KEY (SUBJ_ID) REFERENCES SUBJECT(SUBJ_ID) ON DELETE CASCADE
);
```
### Заполнение таблиц значениями

```sql
INSERT INTO SUBJECT (SUBJ_NAME, HOUR, SEMESTER) 
VALUES 
    ('СИАК', 120, 1),
    ('БД', 100, 2),
    ('ЗИвБТ', 150, 1);

INSERT INTO UNIVERSITY (UNIV_NAME, RATING, CITY) 
VALUES 
    ('БГУИР', 5.0, 'Минск'),
    ('БНТУ', 4.8, 'Минск'),
    ('БелГУТ', 4.5, 'Гомель');

INSERT INTO STUDENT (SURNAME, NAME, STIPEND, KURS, CITY, BIRTHDAY, UNIV_ID) 
VALUES 
    ('Воробьев', 'Илья', 2000.00, 4, 'Москва', '2003-12-27', 1),
    ('Попов', 'Дмитрий', 1500.00, 2, 'Санкт-Петербург', '2000-04-22', 2),
    ('Кузнецова', 'Мария', 1800.00, 3, 'Москва', '2001-02-10', 3);

INSERT INTO EXAM_MARKS (SUBJ_ID, STUDENT_ID, MARK, EXAM_DATE)
VALUES 
    (1, 1, 5.0, '2024-12-14'),
    (2, 2, 3.0, '2024-12-02'),
    (3, 3, 4.0, '2024-12-27');

INSERT INTO LECTURER (SURNAME, NAME, CITY, UNIV_ID) 
VALUES 
    ('Иванов', 'Иван', 'Минск', 1),
    ('Петров', 'Петр', 'Минск', 2),
    ('Сидоров', 'Сидор', 'Острошицы', 3);


INSERT INTO SUBJ_LECT (LECTURER_ID, SUBJ_ID)
VALUES 
    (1, 1),
    (2, 2),
    (3, 3);

```

<hr>

### Просмотр содержимого таблиц

```sql
SELECT * FROM STUDENT;
SELECT * FROM LECTURER;
SELECT * FROM SUBJ_LECT;
SELECT * FROM SUBJECT;
SELECT * FROM UNIVERSITY;
SELECT * FROM EXAM_MARKS;
```
### Удаление таблиц
```sql
DROP TABLE STUDENT;
DROP TABLE LECTURER;
DROP TABLE SUBJ_LECT;
DROP TABLE SUBJECT;
DROP TABLE UNIVERSITY;
DROP TABLE EXAM_MARKS;
```

<hr> 

## вот так все должно будет выглядеть на  сайте
```sql
CREATE TABLE SUBJECT (
    SUBJ_ID SERIAL PRIMARY KEY,
    SUBJ_NAME VARCHAR(100) NOT NULL,
    HOUR INTEGER,
    SEMESTER INTEGER
);

CREATE TABLE UNIVERSITY (
    UNIV_ID SERIAL PRIMARY KEY,
    UNIV_NAME VARCHAR(100) NOT NULL,
    RATING NUMERIC(3, 2),
    CITY VARCHAR(50)
);

CREATE TABLE STUDENT (
    STUDENT_ID SERIAL PRIMARY KEY,
    SURNAME VARCHAR(50) NOT NULL,
    NAME VARCHAR(50) NOT NULL,
    STIPEND NUMERIC(10, 2),
    KURS INTEGER,
    CITY VARCHAR(50),
    BIRTHDAY DATE,
    UNIV_ID INTEGER,
    CONSTRAINT fk_univ_id FOREIGN KEY (UNIV_ID) REFERENCES UNIVERSITY(UNIV_ID)
);

CREATE TABLE EXAM_MARKS (
    EXAM_ID SERIAL PRIMARY KEY,
    SUBJ_ID INTEGER,
    STUDENT_ID INTEGER,
    MARK NUMERIC(3, 1),
    EXAM_DATE DATE,
    CONSTRAINT fk_student FOREIGN KEY (STUDENT_ID) REFERENCES STUDENT(STUDENT_ID) ON DELETE CASCADE,
    CONSTRAINT fk_subject FOREIGN KEY (SUBJ_ID) REFERENCES SUBJECT(SUBJ_ID) ON DELETE CASCADE
);

CREATE TABLE LECTURER (
    LECTURER_ID SERIAL PRIMARY KEY,
    SURNAME VARCHAR(50) NOT NULL,
    NAME VARCHAR(50) NOT NULL,
    CITY VARCHAR(50),
    UNIV_ID INTEGER,
    CONSTRAINT fk_univ_id FOREIGN KEY (UNIV_ID) REFERENCES UNIVERSITY(UNIV_ID)
);

CREATE TABLE SUBJ_LECT (
    LECTURER_ID INTEGER, 
    SUBJ_ID INTEGER,
    PRIMARY KEY (LECTURER_ID, SUBJ_ID),
    CONSTRAINT fk_lecturer_id FOREIGN KEY (LECTURER_ID) REFERENCES LECTURER(LECTURER_ID) ON DELETE CASCADE,
    CONSTRAINT fk_subject FOREIGN KEY (SUBJ_ID) REFERENCES SUBJECT(SUBJ_ID) ON DELETE CASCADE
);

INSERT INTO SUBJECT (SUBJ_NAME, HOUR, SEMESTER) 
VALUES 
    ('СИАК', 120, 1),
    ('БД', 100, 2),
    ('ЗИвБТ', 150, 1);

INSERT INTO UNIVERSITY (UNIV_NAME, RATING, CITY) 
VALUES 
    ('БГУИР', 5.0, 'Минск'),
    ('БНТУ', 4.8, 'Минск'),
    ('БелГУТ', 4.5, 'Гомель');

INSERT INTO STUDENT (SURNAME, NAME, STIPEND, KURS, CITY, BIRTHDAY, UNIV_ID) 
VALUES 
    ('Воробьев', 'Илья', 2000.00, 4, 'Москва', '2003-12-27', 1),
    ('Попов', 'Дмитрий', 1500.00, 2, 'Санкт-Петербург', '2000-04-22', 2),
    ('Кузнецова', 'Мария', 1800.00, 3, 'Москва', '2001-02-10', 3);

INSERT INTO EXAM_MARKS (SUBJ_ID, STUDENT_ID, MARK, EXAM_DATE)
VALUES 
    (1, 1, 5.0, '2024-12-14'),
    (2, 2, 3.0, '2024-12-02'),
    (3, 3, 4.0, '2024-12-27');

INSERT INTO LECTURER (SURNAME, NAME, CITY, UNIV_ID) 
VALUES 
    ('Иванов', 'Иван', 'Минск', 1),
    ('Петров', 'Петр', 'Минск', 2),
    ('Сидоров', 'Сидор', 'Острошицы', 3);


INSERT INTO SUBJ_LECT (LECTURER_ID, SUBJ_ID)
VALUES 
    (1, 1),
    (2, 2),
    (3, 3);

SELECT NAME, SURNAME FROM STUDENT
WHERE SURNAME = 'Воробьев';

SELECT SUBJ_ID
FROM EXAM_MARKS
WHERE EXAM_DATE BETWEEN '2024-12-03' AND '2024-12-26';
```

#### Вывод

```plaintext
Output:

CREATE TABLE
CREATE TABLE
CREATE TABLE
CREATE TABLE
CREATE TABLE
CREATE TABLE
INSERT 0 3
INSERT 0 3
INSERT 0 3
INSERT 0 3
INSERT 0 3
INSERT 0 3
 name | surname  
------+----------
 Илья | Воробьев
(1 row)

 subj_id 
---------
       1
(1 row)
```

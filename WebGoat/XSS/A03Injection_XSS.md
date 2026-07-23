# IDOR

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

# Тестування працездатності системи

*В цьому розділі вказуються засоби тестування, наводяться вихідні коди тестів та результати тестування.*  
*Тестування виконується за допомогою Postman*

## Запуск сервера

![](./photos/server_start.png)

## Тестування GET

### Отримати всіх користувачів

#### Запит:
![](./photos/get_all_tasks.png)

#### Результат:
![](./photos/get_all_tasks_result.png)

### Отримати користувача по id

#### Запит:
![](./photos/get_task_id1.png)

#### Результат:
![](./photos/get_task_id1_result.png)

## Тестування POST

### Додати користувача

#### Запит:
![](./photos/add_task_id4.png)

#### Результат:
![](./photos/add_task_id4_result.png)

#### Перевірка за допомогою GET:
![](./photos/get_task_id4.png)

![](./photos/get_task_id4_result.png)

## Тестування PUT

### Змінити користувача по id

#### Запит:
![](./photos/update_task_4.png)

#### Результат:
![](./photos/update_task_4_result.png)

#### Перевірка за допомогою GET:
![](./photos/get_task_id4_after_update.png)

![](./photos/get_task_id4_after_update_result.png)

## Тестування DELETE

### Видалити користувача по id

#### Запит:
![](./photos/delete_task_4.png)

#### Результат:
![](./photos/delete_task_4_result.png)

#### Перевірка за допомогою GET:
![](./photos/get_all_tasks_after_delete.png)

![](./photos/get_all_users_after_delete_result.png)

![](./photos/get_user_6_after_delete.png)

![](./photos/get_user_6_after_delete_result.png)


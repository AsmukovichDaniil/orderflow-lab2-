# Клієнт через API читає чуже замовлення та інвойс без перевірки власника

**Версія:** OrderFlow 1.1, варіант 2  
**Середовище:** Windows 11, Chromium/Playwright MCP, локальний API http://localhost:5000, ORDERFLOW_VARIANT=2.  
**Область:** GET /api/orders/{id}, GET /api/orders/{id}/invoice  
**Серйозність / пріоритет:** High / P1

## Опис
Після входу як Bob API повертає HTTP 200 для замовлення №1 та інвойсу Alice разом з її адресою, товарними рядками й сумами.

## Кроки відтворення
1. POST /api/auth/login з username=bob, password=bob.
2. GET /api/orders/1.
3. GET /api/orders/1/invoice.
4. Для контролю виконати GET /api/customers/1 у тій самій сесії.
5. Зверніть увагу, що перші два запити повертають HTTP 200 із customerId=1 та Alice Koval, а контрольний запит профілю повертає 403.

## Чому це проблема
Оракул — PRODUCT.md: клієнт може переглядати власні замовлення й інвойси. Позитивна відповідь для об’єкта іншого клієнта порушує межу доступу; контрольний 403 підтверджує, що сесія належить звичайному клієнту Bob.

## Чому це важливо
Клієнт може отримати чуже ім’я, адресу, перелік покупок та суми за відомим чи підібраним ID. Перевірено два маршрути читання; інші операції не досліджувалися.

## Обхідний шлях
Безпечного клієнтського обхідного шляху не підтверджено.

## Вкладення
access-control-result.json із результатами входу, двох чужих GET і контрольного 403.

## API evidence excerpt

POST /api/auth/login returned HTTP 200: username=bob, role=customer, customerId=2. GET /api/orders/1 returned HTTP 200 with id=1, customerId=1, customerName=Alice Koval, plus her order lines and totals. GET /api/orders/1/invoice returned HTTP 200 with billToName=Alice Koval and billToAddress=Khreshchatyk St 22, apt 5, Kyiv, Ukraine. Control request GET /api/customers/1 returned HTTP 403 with an empty body.

# Локальний еквівалент перевірки конвеєра

Remote не налаштовано, отже це локальні результати, а не GitHub Actions run.

| Етап | Коміт | Зміна | Локальний smoke-набір |
|---|---|---|---|
| Базова перевірка | 52f7c7c84a6b92b7a143b31c8bd902dc9af27a92 | Додано restore/build/test у workflow | Успішно: 5 пройшли |
| Знижка | 97490f1bba6b89d05735480bd23aac6c311d1b36 | RegularRate 0.10 → 0.25 | Успішно: 5 пройшли |
| Маршрут | e0e2e58f1d7e766e4e5bab150e5f5b238678b401 | /api/products → /api/catalog | Червоний: 4 пройшли, 1 впав |
| Відкат маршруту | b23551084e9a4fa981134429be1da3513fa309d3 | git revert route rename | Відкотили |
| Відкат знижки | df6ebed0cf14ca03e9458047dbfd57bcd98048d0 | git revert discount rate | Зелений: Build succeeded, 0 warnings/errors; 5 пройшли |

Невдалий тест Application_starts_and_lists_products: запит GET /api/products очікував HTTP 200, фактично 404. Після обох відкатів локальні restore → build → test завершилися успішно.

GitHub Actions не запускався, оскільки URL remote не надано. GitHub run URLs і скриншот журналу віддаленого запуску відсутні.

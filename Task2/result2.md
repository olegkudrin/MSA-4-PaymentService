# Задание 2. Применяем State Machine на практике

Таблица переходов состояний State Machine.

| Исходное состояние | Переходное состояние | Событие                          |
| ------------------ | -------------------- | -------------------------------- |
| INIT               | DEBITING             | Инициация платежа                |
| DEBITING           | CUSTOMER_DEBITED     | Успешное списание средств        |
| DEBITING           | DEBIT_FAILED         | Ошибка списания средств          |
| CUSTOMER_DEBITED   | FRAUD_CHECKING       | Начало проверки на мошенничество |
| FRAUD_CHECKING     | FRAUD_PASSED         | Проверка пройдена успешно        |
| FRAUD_CHECKING     | FRAUD_DETECTED       | Обнаружено мошенничество         |
| FRAUD_CHECKING     | FRAUD_CHECKING       | Ошибка проверки (повтор)         |
| FRAUD_DETECTED     | REFUNDING            | Начало возврата средств          |
| REFUNDING          | CUSTOMER_REFUNDED    | Средства возвращены клиенту      |
| REFUNDING          | REFUNDING            | Ошибка возврата (повтор)         |
| CUSTOMER_REFUNDED  | NOTIFYING_SECURITY   | Начало уведомления безопасности  |
| NOTIFYING_SECURITY | PAYMENT_BLOCKED      | Безопасность уведомлена          |
| NOTIFYING_SECURITY | NOTIFYING_SECURITY   | Ошибка уведомления (повтор)      |
| FRAUD_PASSED       | TRANSFERRING         | Начало перевода контрагенту      |
| TRANSFERRING       | PAYMENT_COMPLETED    | Перевод выполнен успешно         |
| TRANSFERRING       | TRANSFER_FAILED      | Ошибка перевода (необратимая)    |

Начальное состояние - INIT.

Конечные состояния (успех или финальная ошибка):
- PAYMENT_COMPLETED - платеж успешно завершен
- PAYMENT_BLOCKED   - платеж заблокирован из-за мошенничества
- DEBIT_FAILED      - ошибка на этапе списания средств
- TRANSFER_FAILED   - критическая ошибка перевода (требует ручного разбора)

Повторяемые состояния (временная ошибка):
- FRAUD_CHECKING     - ошибка проверки на мошенничество
- REFUNDING          - ошибка возврата
- NOTIFYING_SECURITY - ошибка уведомления

Успешный сценарий:
- INIT
- DEBITING
- CUSTOMER_DEBITED
- FRAUD_CHECKING
- FRAUD_PASSED
- TRANSFERRING
- PAYMENT_COMPLETED

Сценарий обнаружения мошенничества:
- INIT
- DEBITING
- CUSTOMER_DEBITED
- FRAUD_CHECKING
- FRAUD_DETECTED
- REFUNDING
- CUSTOMER_REFUNDED
- NOTIFYING_SECURITY
- PAYMENT_BLOCKED

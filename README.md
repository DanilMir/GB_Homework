# ДЗ 1

## 1 Вариант (Финальный)
```SQL
SELECT 
  DISTINCT sc.country_name, b.battle_id
FROM 
  battles as b
CROSS JOIN ship_classes as sc 
EXCEPT 
SELECT 
  DISTINCT sc.country_name, b.battle_id 
FROM 
  outcomes as o 
  JOIN ships as s ON o.ship_id = s.ship_id 
  JOIN ship_classes as sc ON sc.class_id = s.class_id 
  JOIN battles as b ON b.battle_id = o.battle_id
```

## 2 Вариант (Первоначальный)
Это первоначальный вариант, который был сразу отброшен из-за неэффективности 
```SQL
SELECT 
  DISTINCT cc.country_name, b.* 
FROM 
  battles as b, 
  ship_classes as cc
WHERE 
  cc.country_name NOT IN (
    SELECT 
      DISTINCT sc.country_name 
    FROM 
      outcomes as o 
      JOIN ships as s ON o.ship_id = s.ship_id 
      JOIN ship_classes as sc on sc.class_id = s.class_id 
    WHERE 
      o.battle_id = b.battle_id
  )
```
# ДЗ 2

## 1. Задание

```SQL
SELECT 
  orders.order_id 
FROM 
  order_items 
  JOIN order ON order_items.order_id = orders.order_id 
GROUP BY 
  orders.order_id, 
  product_id 
HAVING 
  SUM(order_items.quantity) > 5
```

## 2. Задание

```SQL
SELECT orders.order_id, 
  SUM(
    CASE
      WHEN orders.order_date >= date_trunc('month', current_date - interval '1' month) 
        AND orders.order_date < date_trunc('month', current_date)
      THEN order_items.quantity ELSE 0 END
  ) AS quantity, 
  SUM(
    CASE
      WHEN orders.order_date >= date_trunc('month', current_date - interval '1' month) 
        AND orders.order_date < date_trunc('month', current_date)
     THEN order_items.quantity * order_items.price ELSE 0 END
  ) AS amount 
FROM 
  order_items 
  JOIN order ON order_items.order_id = orders.order_id 
GROUP BY order_items.product_id
```

## 3. Задание

```SQL
SELECT 
  SUM (
    CASE WHEN status = "Отменён" THEN 1 ELSE 0 END
  ) / COUNT(*) * 100 as percent 
FROM 
  orders 
WHERE 
  order_date >= date_trunc(
    'month', current_date - interval '3' month
  ) 
  AND order_date < date_trunc('month', current_date)
```

## 4. Задание 
```SQL
SELECT 'orders' as table, 'order_id' as column, 'Заказ без товаров' as reason FROM orders
	LEFT JOIN order_items ON orders.order_id = order_items.order_id
    WHERE order_items.order_id IS NULL
UNION
SELECT 'order_items' as table, 'order_id' as column, 'Товар без заказов' as reason FROM orders
	RIGHT JOIN order_items ON orders.order_id = order_items.order_id
    WHERE orders.order_id IS NULL
UNION
SELECT 'orders' as table, 'customer_id' as column, 'У заказа отсутсвует клиент' as reason FROM orders 
    WHERE orders.customer_id IS NULL
UNION
SELECT 'order_items' as table, 'product_id' as column, 'У заказа не указан товар' as reason FROM order_items 
    WHERE order_items.product_id IS NULL
UNION
SELECT 'orders' as table, 'order_date' as column, 'Заказ из будущего' as reason FROM orders
    WHERE orders.order_date > CURRENT_DATE
UNION
SELECT 'orders' as table, 'status' as column, 'Некорректное значение статуса заказа' as reason FROM orders
    WHERE orders.status = "" OR orders.status IS NULL
UNION
SELECT 'order_items' as table, 'quantity' as column, 'Некорректное значение кол-ва товара' as reason FROM order_items 
    WHERE order_items.quantity < 0 OR order_items.quantity is NULL
UNION
SELECT 'order_items' as table, 'price' as column, 'Некорректное значение цены за единицу товара' as reason FROM order_items 
    WHERE order_items.quantity < 0 OR order_items.quantity is NULL
UNION
SELECT 'orders' as table, 'order_date' as column, 'У заказа отсутсвует дата заказа' as reason FROM orders 
    WHERE orders.order_date IS NULL
```

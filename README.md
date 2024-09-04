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
SELECT 
  orders.order_id, 
  SUM(quantity) as quantity, 
  SUM(price * quantity) as amount 
FROM 
  order_items 
  JOIN order ON order_items.order_id = orders.order_id 
WHERE 
  orders.order_date >= date_trunc(
    'month', current_date - interval '1' month
  ) 
  AND orders.order_date < date_trunc('month', current_date) 
GROUP BY 
  order_items.product_id
```


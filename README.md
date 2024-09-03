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

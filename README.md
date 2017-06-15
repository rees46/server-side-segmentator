# Сегментатор #

`php 5.5+`
 
## Назначение ## 

## Установка ##

1. Положить файл ServerSideSegmentator.php в папку с вашим проектом и подключить его

## Использование ##

Получение сегмента пользователя в контролере

```php
<?php

// Инициализировать эксперимент
$recSegmentator = new ServerSideSegmentator('recommendations', 3);

// Получить сегмент пользователя
$segment = $recSegmentator->getSegment();


?>
```

Использование в шаблонах:

```html

...

<!-- Сегмент может быть не задан, например для поисковых роботов, поэтому проверяем его наличие -->

<?php if($segment): %>

	<?php if($segment == 'A'): ?>
		<!-- Показать рекомендер подрядчика № 1-->
	<?php endif;?>
	
	<?php if($segment == 'B'): ?>
		<!-- Показать рекомендер подрядчика № 2-->
	<?php endif;?>
	
	<?php if($segment == 'C'): ?>
		<!-- Показать рекомендер подрядчика № 3-->
	<?php endif;?>
	
	<!-- Отправить сегмент в Google Analytics -->
	<javascript>
		ga('set', 'dimension5', '<?php echo $segment ?>');
	</javascript>
	
<?php endif; ?>

...

```

Запись сегмента в оформленный заказ:

```php

<?php

// ... где-то здесь получили order_id заказа
$order_id = 123;

// Добавляем сегмент к заказу
// Считаем, что у заказа может быть несколько экспериментов, поэтому сегменты пишем в отдельную таблицу "order_segments"
$recSegmentator = new ServerSideSegmentator('recommendations', 3);
$segment = $regSegmentator->getSegment();
if($segment) {
	$query = "INSERT INTO order_segments (order_id, segment) VALUES (" . $order_id . ", '" . $segment . "');";
	mysql_query($segment);
}

?>


```
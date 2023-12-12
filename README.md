## Домашнее задание к занятию «SQL. Часть 2»
Инструкция по выполнению домашнего задания
1. Сделайте fork репозитория c шаблоном решения к себе в Github и переименуйте его по названию или номеру занятия, например, https://github.com/имя-вашего-репозитория/gitlab-hw или https://github.com/имя-вашего-репозитория/8-03-hw).
2. Выполните клонирование этого репозитория к себе на ПК с помощью команды git clone.
3. Выполните домашнее задание и заполните у себя локально этот файл README.md:
 - впишите вверху название занятия и ваши фамилию и имя;
 - в каждом задании добавьте решение в требуемом виде: текст/код/скриншоты/ссылка;
 - для корректного добавления скриншотов воспользуйтесь инструкцией «Как вставить скриншот в шаблон с решением»;
 - при оформлении используйте возможности языка разметки md. Коротко об этом можно посмотреть в инструкции по MarkDown.
4. После завершения работы над домашним заданием сделайте коммит (git commit -m "comment") и отправьте его на Github (git push origin).
5. Для проверки домашнего задания преподавателем в личном кабинете прикрепите и отправьте ссылку на решение в виде md-файла в вашем Github.
6. Любые вопросы задавайте в чате учебной группы и/или в разделе «Вопросы по заданию» в личном кабинете.
Желаем успехов в выполнении домашнего задания.
________________

Задание можно выполнить как в любом IDE, так и в командной строке.

### Задание 1
Одним запросом получите информацию о магазине, в котором обслуживается более 300 покупателей, и выведите в результат следующую информацию:

фамилия и имя сотрудника из этого магазина;
город нахождения магазина;
количество пользователей, закреплённых в этом магазине.

	 select	concat(sf.first_name , ' ', sf.last_name) as 'LFMname Employee',
		 cy.city,
 		COUNT(cr.customer_id) as 'Count Buyers'		
	 from sakila.store s
 	join sakila.staff sf on sf.store_id = s.store_id 
	 join sakila.customer cr on cr.store_id = s.store_id
	 join sakila.address a on a.address_id = s.address_id 
	 join sakila.city cy on cy.city_id = a.city_id 
	 group by sf.staff_id, cy.city_id 
	 having COUNT(cr.customer_id) > 300;

### Задание 2
Получите количество фильмов, продолжительность которых больше средней продолжительности всех фильмов.

	 select COUNT(f.title)
	 from sakila.film f  
	 where f.`length` > (select AVG(`length`) 
   	                  from sakila.film);

### Задание 3
Получите информацию, за какой месяц была получена наибольшая сумма платежей, и добавьте информацию по количеству аренд за этот месяц.

	 select	t.amount_of_payments,
 		t.month_of_payments,
 		(select count(r.rental_id)
 		from sakila.rental r
 		where DATE_FORMAT(r.rental_date, '%M %Y') = t.month_of_payments) 'count_of_rent'
	 from (
	   select SUM(p.amount) 'amount_of_payments', DATE_FORMAT(p.payment_date, '%M %Y') 'month_of_payments' 
	   from sakila.payment p 
 	  group by DATE_FORMAT(p.payment_date, '%M %Y')) t
	 order by t.amount_of_payments desc  
	 limit 1;

### Дополнительные задания (со звёздочкой*)
Эти задания дополнительные, то есть не обязательные к выполнению, и никак не повлияют на получение вами зачёта по этому домашнему заданию. Вы можете их выполнить, если хотите глубже шире разобраться в материале.

### Задание 4*
Посчитайте количество продаж, выполненных каждым продавцом. Добавьте вычисляемую колонку «Премия». Если количество продаж превышает 8000, то значение в колонке будет «Да», иначе должно быть значение «Нет».

### Задание 5*
Найдите фильмы, которые ни разу не брали в аренду.

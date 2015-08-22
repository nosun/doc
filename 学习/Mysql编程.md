##Mysql 编程

###Mysql游标
Mysql游标cursor不是一个查询，而是查询的一个结果集，用于交互使用，Mysql的游标只能用于存储过程和函数。

####样例语句
		create procedure processorders()  
		begin  
		   declare ordernumbers cursor
		   for
		   select order_num from orders;
		end

- 使用declare ordernumbers cursor for  语句声明一个游标
- open ordernumbers； 打开游标
- close ordernumbers  关闭游标
- fetch ordernumbers into 获取数据

###存储过程
- 查看存储过程
> show procedure status;

返回所有的存储过程；
- 调用存储过程

		call productpricing (@pricelow,  
							 @pricehigh,  
							 @priceaverage);  

- 创建存储过程

		create procedure productpricing()  
		begin  
		select avg(prod_price) as priceavergae  
		from products;  
		end;  

- 删除存储过程

	drop procedure productpricing;  

- 使用参数
一般的，存储过程并不现实结果，而是把结果返回给指定的变量  

		create procedure productpricing(  
			out pl decimal(8,2),  
			out ph decimal(8,2),  
			out pa decimal(8,2)  
		)  
		
		begin  
			select min(prod_price)  
			into pl  
			from products;  
			select max(prod_price)  
			into ph  
			from products;  
			select avg(prod_price)  
			into pa  
			from products;  
		end;  

存储过程的参数允许的数据类型与表中的数据类型相同。
记录集不是允许的类型，因此不能通过一个参数返回多个行和列。

		create procedure ordertotal(
			in onumber int,
			out ototal decimal(8,2)
		)
		begin
			select sum(item_price*quantity)
			from orderitems
			where order_num = onumber
			into ototal
		end;
	
		调用
		
		call ordertotal(20005,@total);
		select @total;

- 分隔符
	delimiter //


#### 我写的存储过称

	DROP PROCEDURE  if EXISTS last_user_record;
	CREATE PROCEDURE last_user_record()
	BEGIN
		DECLARE tag INT;
		DECLARE did VARCHAR(255);
		DECLARE today varchar(255);
		DECLARE cur CURSOR FOR SELECT DISTINCT lps_did,p_event_date FROM aura_log_day_10 where preserve8 !='' and p_event_date != '' and lps_did !=0 GROUP BY lps_did,p_event_date ORDER BY p_event_date asc;
		DECLARE CONTINUE HANDLER FOR NOT FOUND SET tag=1;
		OPEN cur;
		SET tag=0;
		WHILE TAG=0 DO
		FETCH cur INTO did,today;
		INSERT into temp2
		(select * from aura_log_day_10 where lps_did = did  and p_event_date = today ORDER BY preserve8 desc limit 1);
 		END WHILE;
 		CLOSE cur;
 	END

- 修改存储过程
	ALTER PROCEDURE
- 注意变量声明的类型
- 注意游标嵌套过程中 continue handler 重复定义的问题，我还不清楚怎么嵌套使用。
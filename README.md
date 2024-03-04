-- creating a database and using it

create database db1;
use db1;

-- creating table customers

create table customers(
customer_id int primary key,
first_name varchar(10),
last_name varchar(10),
email varchar(30),
address varchar(50));

desc customers;


-- inserting values into customers


insert into customers(customer_id,first_name,last_name,email,address) values
(1,'John','Doe','johndoe@example.com','123 Main St, City'),
(2,'Jane','Smith','janesmith@example.com','456 Elm St, Town'),
(3,'Robert','Johnson','robert@example.com','789 Oak St, Village'),
(4,'Sarah','Brown','sarah@example.com','101 Pine St, Suburb'),
(5,'David','Lee','david@example.com','234 Cedar St, District'),
(6,'Laura','Hall','laura@example.com','567 Birch St, County'),
(7,'Michael','Davis','michael@example.com','890 Maple St, State'),
(8,'Emma','Wilson','emma@example.com','321 Redwood St, Country'),
(9,'William','Taylor','william@example.com','432 Spruce St, Province'),
(10,'Olivia','Adams','olivia@example.com','765 Fir St, Territory');

select * from customers;

-- creating table products

create table products(
product_id int primary key,
name varchar(20),
price decimal,
description varchar(50),
stockQuantity int);

desc products;

alter table products
modify column price decimal(10,2);

-- inserting values into products


insert into products(product_id,name,price,description,stockQuantity) values
(1,'Laptop',800.00,'High-performance laptop', 10),
(2,'Smartphone',600.00,'Latest smartphone',15 ),
(3,'Tablet',300.00,'Portable tablet',20),
(4,'Headphones',150.00,'Noise-canceling',30),
(5,'TV',900.00,'4K Smart TV',5),
(6,'Coffee Maker',50.00,'Automatic coffee maker',25),
(7,'Refrigerator',700.00,'Energy-efficient',10),
(8,'Microwave Oven',80.00,'Countertop microwave',15),
(9,'Blender',70.00,'High-speed blender', 20),
(10,'Vacuum Cleaner',120.00,'Bagless vacuum cleaner',10);

select * from products;

-- creating table cart

create table cart(
cart_id int primary key,
customer_id int,
product_id int,
quantity int,
foreign key(customer_id) references customers(customer_id),
foreign key(product_id) references products(product_id));

desc cart;

-- inserting values into cart


insert into cart(cart_id,customer_id,product_id,quantity) values
(1,1,1,2),
(2,1,3,1),
(3,2,2,3),
(4,3,4,4),
(5,3,5,2),
(6,4,6,1),
(7,5,1,1),
(8,6,10,2),
(9,6,9,3),
(10,7,7,2);

select * from cart;

-- creating table orders

create table orders(
order_id int primary key,
customer_id int,
order_date date,
total_price decimal(10,2),
foreign key(customer_id) references customers(customer_id));

desc orders;


-- inserting values into orders


insert into orders(order_id,customer_id,order_date,total_price) values
(1,1,'2023-01-05',1200.00),
(2,2,'2023-02-10',900.00),
(3,3,'2023-03-15',300.00),
(4,4,'2023-04-20',150.00),
(5,5,'2023-05-25',1800.00),
(6,6,'2023-06-30',400.00),
(7,7,'2023-07-05',700.00),
(8,8,'2023-08-10',160.00),
(9,9,'2023-09-15',140.00),
(10,10,'2023-10-20',1400.00);

select * from orders;


-- creating table order_items

create table order_items(
order_item_id int primary key,
order_id int,
product_id int,
quantity int,
item_amount decimal(10,2),
foreign key(order_id) references orders(order_id),
foreign key(product_id) references products(product_id));

desc order_items;


-- inserting values into order_items


insert into order_items(order_item_id,order_id,product_id,quantity,item_amount) values
(1,1,1,2,1600.00),
(2,1,3,1,300.00),
(3,2,2,3,1800.00),
(4,3,5,2,1800.00),
(5,4,4,4,600.00),
(6,4,6,1,50.00),
(7,5,1,1,800.00),
(8,5,2,2,1200.00),
(9,6,10,2,240.00),
(10,6,9,3,210.00);

select * from order_items;



set sql_safe_updates=0;

-- TASKS --

-- 1. Update refrigerator product price to 800.

update products set price=800 where name='Refrigerator';
select * from products;

-- 2. Remove all cart items for a specific customer.

delete from cart where customer_id=1;

select * from cart;

-- 3.Retrieve Products Priced Below $100.

select * from products where price<100;

-- 4. Find Products with Stock Quantity Greater Than 5.

select * from products where stockQuantity>5;

-- 5. Retrieve Orders with Total Amount Between $500 and $1000.

select * from orders where total_price between 500 and 1000;

-- 6. Find Products which name end with letter ‘r’.

select * from products where name like '%r';

-- 7. Retrieve Cart Items for Customer 5.

select * from cart where customer_id=5;

-- 8. Find Customers Who Placed Orders in 2023.

select * from orders where year(order_date)=2023;

-- 9. Determine the Minimum Stock Quantity for Each Product Category.

select product_id,min(stockQuantity) from products group by product_id;

-- 10. Calculate the Total Amount Spent by Each Customer.

select c.customer_id,c.first_name,c.last_name,SUM(oi.item_amount) AS total_amount_spent from customers c
join orders o on c.customer_id = o.customer_id
join order_items oi on o.order_id = oi.order_id
group by c.customer_id;

-- 11. Find the Average Order Amount for Each Customer.

select c.customer_id,c.first_name,c.last_name,avg(oi.item_amount) as total_amount_spent from customers c
join orders o on c.customer_id = o.customer_id
join order_items oi on o.order_id = oi.order_id
group by c.customer_id;

-- 12. Count the Number of Orders Placed by Each Customer.

select c.customer_id,c.first_name,c.last_name,count(oi.order_id) from customers c
join orders o on c.customer_id=o.customer_id
join order_items oi on o.order_id = oi.order_id
group by c.customer_id;

-- 13. Find the Maximum Order Amount for Each Customer.

select c.customer_id,c.first_name,c.last_name,max(oi.item_amount) from customers c
join orders o on c.customer_id=o.customer_id
join order_items oi on o.order_id = oi.order_id
group by c.customer_id;

-- 14. Get Customers Who Placed Orders Totaling Over $1000.

select c.customer_id,c.first_name,c.last_name,sum(oi.item_amount) amount from customers c
join orders o on c.customer_id=o.customer_id
join order_items oi on o.order_id = oi.order_id
group by c.customer_id
having amount>1000;

-- 15. Subquery to Find Products Not in the Cart.

select * from cart;
 
select * from products where product_id not in(select product_id from cart);

-- 16. Subquery to Find Customers Who Haven't Placed Orders.

select * from customers where customer_id not in(select customer_id from order_items);

-- 17. Subquery to Calculate the Percentage of Total Revenue for a Product.

select product_id,(sum(item_amount)/ (select sum(total_price) from orders)) * 100 as tot_rev_percent from order_items
group by product_id;
 
-- 18. Subquery to Find Products with Low Stock.

select * from products where stockQuantity < (select avg(stockQuantity) from products);

select * from products;

-- 19. Subquery to Find Customers Who Placed High-Value Orders.

select * from customers where customer_id in(select customer_id from orders where total_price>(select avg(total_price) from orders));

# MySqlTask

create database ecommerce;
use ecommerce;
create table customers(
	id int unique primary key auto_increment,
    name varchar(100),
    email varchar(100),
    address varchar(100)
);
create table orders(
	id int unique primary key auto_increment,
    customer_id int,
    order_date date,
    total_amount int,
    foreign key(customer_id) references customers(id)
);
create table products(
	id int unique primary key auto_increment,
    name varchar(100),
    price decimal(8,2),
    description varchar(100)
    );
    insert into customers(name,email,address) values
    ('Gnanavel','gnanavel@gmail.com','Bengaluru'),
    ('Gopal','gopal@yahoo.com','Namakkal'),
    ('Lavanya','lavanya@outlook.com','Coimbatore'),
    ('Udhaya','udhaya@gmail.com','Chitambaram');
    select * from customers;
    
    insert into products(name,price,description) values
   ('Product A', 50.00, 'Description for Product A'),
('Product B', 30.00, 'Description for Product B'),
('Product C', 40.00, 'Description for Product C'),
('Product D', 60.00, 'Description for Product D');
   select * from products;
   
   insert into orders(customer_id,order_date,total_amount) values
   (1, '2025-02-10', 150.00),
(2, '2025-02-20', 120.00),
(1, '2025-03-01', 200.00),
(3, '2025-03-05', 300.00);
select * from orders;

-- Tasks--  
-- Update the price of Product C to 45.00.
update products set price=45.00 where name='Product C';

-- Add a new column discount to the products table
alter table products add column discount tinyint;
select * from products;

-- Retrieve the top 3 products with the highest price.
select name,price from products order by price desc limit 3;

-- Retrieve the orders with a total amount greater than 150.00.
select * from orders where total_amount>150.00;

-- Retrieve the average total of all orders.
select avg(total_amount) as 'Average Of Orders' from orders;

-- Retrieve all customers who have placed an order in the last 30 days.
select distinct name,email from customers
join orders on orders.customer_id=customers.id
where orders.order_date>=curdate() -interval 30 day;

-- Get the total amount of all orders placed by each customer.
select name,sum(total_amount) as 'Sum' from customers
join orders on orders.customer_id=customers.id
group by customers.id;

-- Join the orders and customers tables to retrieve the customer's name and order date for each order.
select distinct name,order_date from customers
join orders on orders.customer_id=customers.id;

-- Normalize the database by creating a separate table for order items and updating the orders table to reference the order_items table.
create table order_items(
	id int unique primary key auto_increment,
	order_id int,
    product_id int,
    quantity int,
    price decimal(8,2),
    foreign key (order_id) references orders(id),
    foreign key(product_id) references products(id)
);
insert into order_items(order_id,product_id,quantity,price) values
(1,1,2,300.00),
(2,2,1,120.00),
(1,3,3,150.00),
(3,1,2,200.00);
select * from order_items;

-- Get the names of customers who have ordered Product A.
select distinct customers.name from customers
join orders on orders.customer_id=customers.id
join order_items on order_items.order_id=orders.id
join products on products.id=order_items.product_id
where products.name='Product A';


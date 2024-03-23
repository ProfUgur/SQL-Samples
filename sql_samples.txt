--1 Write a query to retrieve product names (`ProductName`) and quantity per unit (`QuantityPerUnit`)
Select product_name, quantity_per_unit from products;

--2 Write a query to get the values of Product Numbers (`ProductID`) and Product names (`ProductName`). Filter out products that are no longer sold (`Discontinued`)
Select product_id, product_name from products
where discontinued = 0;

--3 Write a query to retrieve the Discontinued Product List with values for Product ID and name (`ProductID`, `ProductName`)
Select product_id, product_name from products
where discontinued = 1;

--4 Write a query to retrieve the list of Products (`ProductID`, `ProductName`, `UnitPrice`) whose products cost less than 20
Select product_id,product_name,unit_price from products
where unit_price < 20;

--5 Write a query to retrieve the list of Products (`ProductID`, `ProductName`, `UnitPrice`) where the products cost between 15 and 25
Select product_id,product_name, unit_price from products
where unit_price between 15 and 25;

--6 Write a query to retrieve the product list (`ProductName`, `UnitsOnOrder`, `UnitsInStock`) when the stock is less than the quantity in the order
Select product_name, units_on_order, units_in_stock from products
where units_on_order > units_in_stock;

--7 List the products whose names start with 'a'
Select product_name from products
where lower(product_name) like 'a%';

--8 List the products whose names end with `i`
Select product_name from products
where lower(product_name) like '%i';

--9. Write a query to get the list of products (ProductName, UnitPrice, UnitPriceKDV) by adding 18% VAT to the unit prices
Select product_name, unit_price, (unit_price * 1.18) as value_added_tax
from products;

--10 How many products are there with prices greater than 30?
Select count(*) from products
where unit_price > 30;

--11 Make the names of the products completely smaller and list them in reverse order by price
Select lower(product_name), unit_price from products
order by unit_price desc;

--12 Print the names and surnames of the employees side by side
Select concat(first_name,' ', last_name) as first_last_name from employees;

--13 How many suppliers do I have whose Region field is NULL?
Select count(company_name) from suppliers
where region is null;

--14 How many suppliers do I have whose Region field is not NULL?
Select count(company_name) from suppliers
where region is not null;

--15 Put TR to the left of all product names and enlarge them and print them on the screen
Select concat('TR',upper(product_name)) from products;

--16 Add TR to the beginning of the name of the products whose price is less than 20
Select concat('TR',product_name), unit_price from products
where unit_price < 20
order by unit_price asc;

--17 Write a query to retrieve the most expensive product list (`ProductName` , `UnitPrice`)
Select product_name, unit_price from products
where unit_price = (Select max(unit_price) from products);

--18 Write a query to retrieve the Product list (`ProductName` , `UnitPrice`) of the ten most expensive products
Select product_name, unit_price from products
order by unit_price desc
limit 10;

--19 Write a query to retrieve the list of Products (`ProductName` , `UnitPrice`) above the average price of the products
Select product_name, unit_price from products
where unit_price > (select avg(unit_price) from products)
order by unit_price desc;

--20 What is the amount obtained when the products in stock are sold?
Select product_name,units_in_stock,units_on_order,sum(units_in_stock - units_on_order) as "difference" from products
group by product_id
order by product_name;

--21 Write a query to get counts of Existing and Discontinued products
Select count(units_in_stock) as "current_discontinued" from products
where discontinued = 1;

--22 Write a query to retrieve the products along with their category names
Select product_name, category_name from products
inner join categories on products.category_id = categories.category_id
order by product_name;

--23 Write a query to average the price of products across categories
Select category_name,avg(unit_price) as "average" from products
inner join categories on products.category_id = categories.category_id
group by categories.category_name
order by categories.category_name;

--24 What is the name, price and category name of my most expensive product?
Select product_name, category_name, unit_price from products
inner join categories on products.category_id = categories.category_id
where unit_price = (Select max(unit_price) from products);

--25 Name of its best-selling product, name of its category and name of its supplier
Select product_name, category_name, company_name, units_on_order from products
inner join categories on products.category_id = categories.category_id
inner join suppliers on products.supplier_id = suppliers.supplier_id
where units_on_order = (Select max(units_on_order) from products);

--26 Write a query to retrieve the product list of out-of-stock products along with the suppliers' name and contact number (`ProductID`, `ProductName`, `CompanyName`, `Phone`)
select product_id,product_name,company_name,phone from products p 
inner join suppliers s on p.supplier_id=s.supplier_id
where units_in_stock=0;

--27 Address of my orders in March 1998, name of the employee who received the order, surname of the employee
select first_name|| ' ' || last_name as "first_last_name",address,order_date from orders o 
inner join employees e on o.employee_id=e.employee_id 
where date_part('year',o.order_date)=1998 
and date_part('month',o.order_date)=03;

--28 How many orders do I have in February 1997?
select o.order_date,sum(od.quantity) as total_quantity from orders o
inner join order_details od on o.order_id=od.order_id
where date_part('year',o.order_date)=1997
group by o.order_date;

--29 How many orders do I have from London in 1998?
select ship_city,sum(quantity) as total from orders o
inner join order_details od on o.order_id=od.order_id 
where date_part('year',o.order_date)=1998 and ship_city in ('London')
group by ship_city;

--30 Contactname and phone number of my customers who placed orders in 1997
select distinct contact_name,phone from customers c
inner join orders o on c.customer_id=o.customer_id
where date_part('year',o.order_date)=1997;

--31 My orders with transportation fee over 40
select order_id,freight from orders where freight > 40 order by freight;

--32 The city and name of the customer for my orders with a transportation fee of 40 and above
select company_name ,ship_city,freight from customers c
inner join orders o on c.customer_id=o.customer_id
where freight >=40 order by freight;

--33 Date, city, employee name and surname (name and surname will be combined and capitalized) of the orders placed in 1997
select order_date,ship_city,concat(upper(first_name),' ',upper(last_name))from orders o 
inner join employees e on o.employee_id=e.employee_id
where date_part('year',o.order_date)=1997;

--34 Contactname and phone numbers of customers who placed orders in 1997 (phone format should be like 2223322)
select c.contact_name,TRIM(REPLACE(REPLACE(REPLACE(REPLACE(c.phone,'(',' '),')',' '),' ',' '),'.',' '),'-'',') from orders o
inner join customers c on o.customer_id = c.customer_id
where date_part('year',o.order_date) = 1997;

--35 Order date, customer contact name, employee name, employee surname
select c.contact_name,o.order_date,e.first_name,e.last_name from orders o 
inner join employees e on o.employee_id=e.employee_id 
inner join customers c on o.customer_id=c.customer_id;

--36 My delayed orders?
Select order_id from orders
where required_date < shipped_date;

--37 Date of my delayed orders, name of customer
Select order_date,company_name from orders o
inner join customers c on o.customer_id = c.customer_id
where required_date < shipped_date;

--38 Name, category name and quantity of products sold in order no. 10248
Select order_id, product_name, category_name, quantity from products p
inner join order_details od on od.product_id = p.product_id
inner join categories c on c.category_id = p.category_id
where od.order_id = 10248;

--39 Name of the products of order no. 10248, supplier name
Select order_id, product_name, company_name from products p
inner join order_details od on od.product_id = p.product_id
inner join suppliers s on s.supplier_id = p.supplier_id
where od.order_id = 10248;

--40 Name and quantity of products sold by the employee with ID number 3 in 1997
Select employee_id, product_name, quantity, order_date from order_details od
inner join products p on od.product_id = p.product_id
inner join orders o on od.order_id = o.order_id
where date_part('year',o.order_date)=1997 and o.employee_id = 3;

--41 Once in 1997, the ID, name and surname of my best-selling employee
Select e.employee_id, concat(first_name,' ',last_name), quantity, order_date from orders o
inner join employees e on e.employee_id = o.employee_id
inner join order_details od on od.order_id = o.order_id
where date_part('year',o.order_date)=1997 
and quantity = (Select max(quantity) from order_details);

--42 ID, Name and Surname of my employee who made the most sales in 1997 ****
select e.employee_id,e.first_name,e.last_name,sum(od.quantity) as total_sale from employees e
inner join orders o on e.employee_id=o.employee_id
inner join order_details od on o.order_id=od.order_id
where date_part('year',o.order_date)=1997
group by e.employee_id
order by total_sale desc limit 1;

--43 What is the name, price and category of my most expensive product?
select product_name,unit_price,category_name from categories c 
inner join products p on c.category_id=p.category_id
where unit_price=(select max(unit_price) from products);

--44 Name and surname of the personnel taking the order, order date, order ID. Sort by order date
select o.order_id,o.order_date,e.first_name,e.last_name from orders o
inner join employees e on o.employee_id=e.employee_id
order by o.order_date;

--45 What is the average price and orderid of my LAST 5 orders?
select o.order_id,o.order_date,avg(od.unit_price) as average from orders o
inner join order_details od on o.order_id=od.order_id
group by o.order_id
order by o.order_date desc limit 5;

--46 What is the name and category of my products sold in January and what is the total sales amount?
select p.product_name,c.category_name,o.order_date,sum(od.unit_price*od.quantity) as total from products p 
inner join categories c on p.category_id=c.category_id
inner join order_details od on p.product_id=od.product_id
inner join orders o on od.order_id=o.order_id
where date_part('month',o.order_date)=01
group by p.product_name,c.category_id,o.order_date;

--47 What are my sales above my average sales amount?
select*from order_details
where (select avg(unit_price*quantity)from order_details)<(unit_price*quantity);

--48 Name of my best-selling product (by quantity), name of its category and name of its supplier
select product_name,category_name,company_name,quantity from products p 
inner join order_details od on p.product_id=od.product_id
inner join categories c on p.category_id=c.category_id
inner join suppliers s on p.supplier_id=s.supplier_id
order by quantity desc limit 1;

--49 How many countries do I have customers from?
select count(distinct country) from customers;

--50 How many products in total has the employee with ID number 3 sold since last January?
select e.employee_id, sum(od.quantity)  from orders o
inner join order_details od on od.order_id = o.order_id
inner join employees e on e.employee_id = o.employee_id
where e.employee_id = 3
  AND o.order_date >= '1998-01-01'
  AND o.order_date <= CURRENT_DATE
GROUP BY e.employee_id;

--Between 51-64 => The same questions were asked from question 38 to question 50

--65 How much turnover did I generate from my product with ID number 10 in the last 3 months?
Select p.product_id, p.product_name, sum(od.quantity*od.unit_price -od.discount) as ciro from
products p 
inner join order_details od ON od.product_id = p.product_id
inner join orders o ON o.order_id = od.order_id
where p.product_id=10
 AND o.order_date >= '1998-02-04'
 AND o.order_date <= '1998-05-04'
group by p.product_id, p.product_name;

--66 How many total orders has each employee received so far?
Select concat(e.first_name, ' ', e.last_name), e.employee_id, Count(order_id) from orders o 
inner join employees e on o.employee_id = e.employee_id
group by concat(e.first_name, ' ', e.last_name),e.employee_id;

--67 I have 91 customers. Only 89 of them placed an order. Find 2 people who didn't order
Select company_name from customers c
left join orders o on o.customer_id = c.customer_id
where order_id is null or order_id = 0;

--68 Company Name, Representative Name, Address, City, Country information of customers in Brazil
Select company_name, contact_name, address, city, country from customers
where country in ('Brazil');

--69 Non-Brazil customers
Select company_name, country from customers
where country not in ('Brazil');

--70 Customers whose country is EITHER Spain, France, or Germany
Select company_name, country from customers
where country in ('Spain','France','Germany');

--71 Customers whose fax numbers I do not know
Select company_name, fax from customers
where fax is null;

--72 My clients in London or Paris
Select company_name, city from customers
where city = 'London' or city = 'Paris';

--73 Customers who both reside in Mexico D.F. AND whose ContactTitle is 'owner'
Select company_name,contact_title, city from customers
where city = 'México D.F.' and contact_title = 'Owner';

--74 Names and prices of my products starting with C
Select product_name, unit_price from products
where product_name like 'C%';

--75 Employees (Employees) whose names (FirstName) start with the letter 'A'; Name, Surname and Dates of Birth
Select Concat(first_name, ' ', last_name), birth_date from employees
where first_name like 'A%';

--76 Company names of my customers with 'RESTAURANT' in their names
Select upper(company_name) from customers
where upper(company_name) like '%RESTAURANT%';

--77 Names and prices of all products between $50 and $100
Select product_name, unit_price from products
where unit_price between 50 and 100;

--78 OrderID and OrderDate information of orders between 1 July 1996 and 31 December 1996
Select order_id, order_date from orders
where order_date between '01/07/1996' and '31/12/1996';

--79 --> Same as question 70

--80 --> Same as question 71

--81 I list my customers by country
Select company_name, country from customers
order by country;

--82 Sort my products from most expensive to cheapest, as a result we want the product name and price
Select product_name, unit_price from products
order by unit_price desc;

--83 Let it list my products from the most expensive to the cheapest, but show its stocks from smallest to largest. As a result, we want the product name and price
Select product_name, unit_price, units_in_stock from products
order by unit_price desc, units_in_stock asc;

--84 How many products are in the No. 1 category..?
Select count(*) from products p
inner join categories c on c.category_id = p.category_id
where c.category_id = 1;

--85 How many different countries do I export to?
Select count(distinct country) from customers;
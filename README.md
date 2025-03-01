# AXON: A PREMIER RETAILER OF CLASSIC CARS!

<p align="center">
  <img src="https://github.com/user-attachments/assets/f5841f31-15dc-4017-bfdc-6f88a7a76378" alt="HD Wallpaper - 1967 Ford Mustang">
</p>

# **Introduction to Axon**  

Axon is a well-established company specializing in high-quality collectible models, catering to enthusiasts and collectors worldwide. The company offers a diverse range of products, including Classic Cars, Motorcycles, Trains, Ships, Planes, Trucks & Buses, and Vintage Cars.


# **About the Company**

Axon is known for its commitment to excellence, ensuring top-tier product quality and exceptional customer service. The company has built strong relationships with trusted vendors and suppliers to maintain a steady product supply. With a customer-centric approach, Axon focuses on efficient order management, timely deliveries, and seamless transactions. Its growing market presence and data-driven strategies enable sustainable growth and continuous expansion into new regions.


# **The MySQL sample database schema consists of the following 8 tables, each designed to store specific data relevant to the retail business:**



<p align="center">
  <img src="https://github.com/user-attachments/assets/b50e0e3e-6856-453d-99a0-f8cdc3c68d47" alt="Axon Image">
</p>


# *1.Customers with the Highest Number of Products Purchased!*

select c.customerName , count(o.orderNumber) as Total_orders

from customers as c

join orders as o on c.customerNumber=o.customerNumber

group by customerName

order by Total_orders desc

limit 10;

![Screenshot 2025-02-11 231459](https://github.com/user-attachments/assets/50f623b6-5278-4de2-96ac-1d32be339732)

#  *2.Where Are the Majority of Our Customers Located?!*

select country, count(*) as total_customers 
from customers

group by country

order by total_customers desc 

limit 5;

![Screenshot 2025-02-11 231931](https://github.com/user-attachments/assets/355fc4be-a7fd-429d-88e0-e761e31a2a6c)

# *3.How Are Customers Categorized Based on Their Credit Limits?*


select

    CASE 
    
        WHEN creditLimit >= 100000 THEN 'Platinum (100,000 & above)'
        
        WHEN creditLimit BETWEEN 50000 AND 99999 THEN 'Gold (50,000 - 99,999)'
        
        WHEN creditLimit BETWEEN 20000 AND 49999 THEN 'Silver (20,000 - 49,999)'
        
        ELSE 'Bronze (Below 20,000)'
        
    END as creditCategory,
    
    COUNT(*) as customerCount
   
    from customers

group by creditCategory

order by customerCount desc;


![Screenshot 2025-02-11 232438](https://github.com/user-attachments/assets/e040b8ec-bda2-4a54-9f29-f019df2b3dd6)

# *4.Top 10 Best-Selling Products by Quantity Ordered!*

select productName,sum(quantityOrdered) as TotalQuantityOrdered

from orderdetails as od

join products p on od.productCode = p.productCode

group by productName

order by TotalQuantityOrdered desc

limit 10;

![Screenshot 2025-02-11 233456](https://github.com/user-attachments/assets/3cfa6bb0-ab9f-4b04-8011-394e18609085)

# *5.Tracking the Number of Orders Placed Annually*


select year(orderDate) year, count(orderDate)  total_orders

from orders

group by year(orderDate);


![Screenshot 2025-02-11 234444](https://github.com/user-attachments/assets/8125b663-a791-488c-8b1b-407d202c7dfc)


# *6.How Many Customers Does Each Sales Representative Handle?*


select e.employeeNumber, concat(firstname,' ',lastname) as Full_Name,count(*) Total_customers  

from customers c

join employees as e on c.salesRepEmployeeNumber= e.employeeNumber

where salesRepemployeenumber is not null

group by 1 

order by Total_customers desc;


![Screenshot 2025-02-11 234942](https://github.com/user-attachments/assets/35dd6087-2371-4774-a89e-afc503389284)

# *7.Could you show the product-wise inventory remaining in the Classic warehouse?*

with cte as(

           select p.productcode,productname,quantityinstock,sum(quantityordered) as quantityordered
           from products as p 
           join orderdetails as od on p.productCode=od.productCode
           group by p.productCode,productName,quantityInStock
           order by quantityordered desc)
           
select productcode,productName,quantityinstock,(quantityinstock - (quantityordered)) as Remained_Stock from cte 

order by Remained_stock desc;

![Screenshot 2025-02-12 000115](https://github.com/user-attachments/assets/0c428710-d09f-4825-9882-770866bc078b)

# *8.What is the breakdown of orders by their delivery status, including count and percentage?*

select  status as Order_Status, 

    COUNT(DISTINCT orderNumber) as Order_Count,
    
    CONCAT(ROUND((COUNT(DISTINCT orderNumber) * 100.0) / 
    
            (select COUNT(DISTINCT orderNumber) from orders), 
            2
        ), '%'
    ) as Order_Percentage

from orders

group by status

order by Order_Count desc;

![Screenshot 2025-02-12 002855](https://github.com/user-attachments/assets/abf4d8e8-5c59-4699-9f05-3787313e4823)

# *9.Number of Products Ordered per Vendor*


select p.productVendor as Vendor_Name, COUNT(od.orderNumber) as Total_Orders

from products p

join orderdetails od on  od.productCode = p.productCode

group by productVendor

order by Total_Orders DESC;


![Screenshot 2025-02-12 001733](https://github.com/user-attachments/assets/43aa1270-bf6d-434b-93a3-7e3f49957ced)


# *10. Product-wise Profit Margin Report*

select  productLine as Category,

    SUM(quantityInStock) as Total_Stock,
    SUM(quantityOrdered) as Items_Sold,
    SUM(quantityInStock) - SUM(quantityOrdered) as Stock_Remaining,
    AVG(buyPrice) as Avg_Cost_Price,
    AVG(priceEach) as Avg_Sale_Price,
    SUM(quantityOrdered * priceEach) as Revenue,
    SUM(quantityOrdered * buyPrice) as Total_Expense,
    SUM(quantityOrdered * priceEach) - SUM(quantityOrdered * buyPrice) as Net_Profit,
    CONCAT(ROUND((SUM(quantityOrdered * priceEach) - SUM(quantityOrdered * buyPrice))  / NULLIF(SUM(quantityOrdered * priceEach), 0) * 100, 2), '%'
    ) as Profit_Percentage from products p
    
join orderdetails od on p.productCode = od.productCode
group by productLine

order by Profit_percentage desc;


![Screenshot 2025-02-12 001410](https://github.com/user-attachments/assets/af3de7fe-307e-4f1d-8d50-6a799a8e8a92)





# *POWER BI DASHBOARD OF AXON SALES*

![AXON_SALES_REPORT_pages-to-jpg-0001](https://github.com/user-attachments/assets/6b048cfc-95e5-4015-8644-43bfa61e34d0)


![AXON_SALES_REPORT_pages-to-jpg-0002](https://github.com/user-attachments/assets/bd949c42-552b-46c5-b198-459ad35f3df3)


![AXON_SALES_REPORT_pages-to-jpg-0003](https://github.com/user-attachments/assets/d3a7bb70-aad2-4af8-b36e-b171684d7a1e)


![AXON_SALES_REPORT_pages-to-jpg-0004](https://github.com/user-attachments/assets/84db006c-96cf-4040-99d4-99db1e11bab8)


![AXON_SALES_REPORT_pages-to-jpg-0005](https://github.com/user-attachments/assets/5ea2cd91-039e-4671-8c48-425a284c8148)







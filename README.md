# MoneySmart
SQL Coding Exam
/*Write an SQL statement to find the total number of user sessions each page has each day.
select TO_TIMESTAMP(concat(visit_date, concat(' ',Visit_time)),'DD/mm/yyyy HH:MI:SS') tim,a.* from samplepage a*/

SELECT
    page_id,
    visit_date,
    COUNT(id) total_user_sessions  --Taking the count of the ID for distinct user sessions.
FROM
    samplepage
GROUP BY
    page_id,
    visit_date
ORDER BY page_id


/*1. Which month has the highest sales? Is there any seasonality effect?*/
/*In the month of Nov there are more Sales. It could be because of  year end sales/ Christmas/ Thanks Giving sales */

SELECT
    TO_CHAR(
        TO_DATE(
            orderdate,
            'dd/mm/yy'
        ),
        'MON'
    ) order_month,
    round(
        SUM(sales),
        0
    ) sales
FROM
    sample_orders
GROUP BY
    TO_CHAR(
        TO_DATE(
            orderdate,
            'dd/mm/yy'
        ),
        'MON'
    )
ORDER BY sales DESC
 
 
 
 /*2. Which product is the recent best seller?*/
 -- According to the below output, In terms of quantity and Sales Amount. Product : Kensington 7 Outlet MasterPiece HOMEOFFICE Power Control Center fairs better.
 -- According to the numbers they have sold about 13 pieces.
SELECT
    *
FROM
    (
        SELECT
            TO_DATE(
                orderdate,
                'dd/mm/yy'
            ) orddate,
            TO_CHAR(
                TO_DATE(
                    orderdate,
                    'dd/mm/yy'
                ),
                'MON-YY'
            ) AS order_date,
            productid,
            productname,
            round(
                SUM(quantity),
                0
            ) quantity,
            round(
                SUM(sales),
                0
            )
        FROM
            sample_orders
        WHERE
            TO_CHAR(
                TO_DATE(
                    orderdate,
                    'dd/mm/yy'
                ),
                'MON-YY'
            ) IN (
                'DEC-17'
            ) --and productid = 'FUR-FU-10004270'
        GROUP BY
            TO_DATE(
                orderdate,
                'dd/mm/yy'
            ),
            TO_CHAR(
                TO_DATE(
                    orderdate,
                    'dd/mm/yy'
                ),
                'MON-YY'
            ),
            productid,
            productname
        ORDER BY quantity DESC
    )
WHERE
    ROWNUM <= 10
 
 /*3. Is there any group of products which are often bought together?*/
 /* As per the below query  I donot see any combination of the product sold often.*/
 
SELECT
    products,
    COUNT(*)
FROM
    (
        SELECT
            orderid,
            LISTAGG(
                productid,
                ','
            ) WITHIN GROUP(ORDER BY productid) products
        FROM
            sample_orders
        GROUP BY
            orderid
    )
GROUP BY
    products
ORDER BY 2
 
/*4.is there any other insights you can suggest to improve sales numbers?
-- Running more campaigns/offers to attract more buyers in the Technology product segment. According to the data,for technology related products the volumes are less
-- But If we can increase these volumes then we can increase the overall turn over*/

SELECT SUBSTR(PRODUCTID,1,3) Product_Cat,COUNT(QUANTITY) Volume, SUM(SALES)  Sales
FROM  sample_orders
group by SUBSTR(PRODUCTID,1,3)
order by COUNT(QUANTITY)
 
 /* 
 5. Based on the data we have, what kind of BI dashboards you would build in order to help the sales
team monitoring the performance? */
-- Provided screen shot of the report developed.
-- We can build reports to display the sales/volume trend over the years.
-- Top 10 Customers based on Sales
-- Top 10 Products Based on Sales and Volume
-- Best Performing Product type like OFF(Office Products), TEC(Technology Products), FUR(Furniture Products) in terms of Volume and Sales.
-- Worst performing Products
-- Year on Year comparision of products based volume and sales
-- Month on Month comparision of products based volume and sales

 
 

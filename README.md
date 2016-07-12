# SQLITE

1. How many users are there?
  - 50 users
  - Found using: SELECT COUNT( * ) FROM users;

2. What are the 5 most expensive items?
  - 25|Small Cotton Gloves|Automotive, Shoes & Beauty|Multi-layered modular service-desk|9984
    83|Small Wooden Computer|Health|Re-engineered fault-tolerant adapter|9859
    100|Awesome Granite Pants|Toys & Books|Upgradable 24/7 access|9790
    40|Sleek Wooden Hat|Music & Baby|Quality-focused heuristic info-mediaries|9390
    60|Ergonomic Steel Car|Books & Outdoors|Enterprise-wide secondary firmware|9341
  - Found using: SELECT * FROM items ORDER BY price DESC LIMIT 5;

3. What's the cheapest book? (Does that change for "category is exactly 'book'" versus "category contains 'book'"?)
  - 76|Ergonomic Granite Chair|Books|De-engineered bi-directional portal|1496
  - Found using: SELECT * FROM items WHERE category = 'Books' ORDER BY price ASC LIMIT 1;
      is the same as;  
  - 76|Ergonomic Granite Chair|Books|De-engineered bi-directional portal|1496
  - Found using: SELECT * FROM items WHERE category LIKE '%Books%' ORDER BY price ASC LIMIT 1;

4. Who lives at "6439 Zetta Hills, Willmouth, WY"? Do they have another address?
  - 40|Corrine|Little|rubie_kovacek@grimes.net|43|40|6439 Zetta Hills|Willmouth|WY|15029
  - Found using: SELECT * FROM users INNER JOIN addresses WHERE users.id = addresses.user_id AND addresses.street = '6439 Zetta Hills
  - 43|40|6439 Zetta Hills|Willmouth|WY|15029|40|Corrine|Little|rubie_kovacek@grimes.net
    44|40|54369 Wolff Forges|Lake Bryon|CA|31587|40|Corrine|Little|rubie_kovacek@grimes.net
  - Found using: SELECT * FROM addresses INNER JOIN users WHERE users.first_name = 'Corrine' AND users.last_name = 'Little' AND users.id = addresses.user_id;  

5. Correct Virginie Mitchell's address to "New York, NY, 10108".
  - 41|39|12263 Jake Crossing|New York|NY|10108|39|Virginie|Mitchell|daisy.crist@altenwerthmonahan.biz
    42|39|83221 Mafalda Canyon|New York|NY|10108|39|Virginie|Mitchell|daisy.crist@altenwerthmonahan.biz
  - Found using: UPDATE addresses SET city = 'New York', state = 'NY', zip = '10108' WHERE user_id IN (SELECT id FROM users WHERE first_name = 'Virginie' AND last_name = 'Mitchell');  

6. How much would it cost to buy one of each tool?
  - 46477
  - Found using: SELECT SUM(price) FROM items WHERE category LIKE '%Tool%';

7. How many total items did we sell?
  - 2125
  - Found using: SELECT SUM(quantity) FROM orders;

8. How much was spent on books?
  - 1081352
  - Found using: SELECT SUM(price * quantity) FROM items INNER JOIN orders WHERE orders.item_id = items.id AND category LIKE '%Books%';

9. Simulate buying an item by inserting a User for yourself and an Order for that User.
  - 51|Sean|Ritter|Rittersm@gmail.com
  - Done with: INSERT INTO users VALUES (null, 'Sean', 'Ritter', 'Rittersm@gmail.com');
  - 378|51|27|100|2016-07-11 14:41:33.307689
  - Done with: INSERT INTO orders VALUES (null, '51', '27', '100', '2016-07-11 14:41:33.307689');

<!-- Adventure Mode -->

10. What item was ordered most often? Grossed the most money?
  - 126|Fantastic Steel Shirt
  - Found using: SELECT SUM(orders.quantity), items.title FROM items INNER JOIN orders  WHERE items.id = orders.item_id GROUP BY orders.item_id ORDER BY SUM(orders.quantity) DESC LIMIT 1;
  - 534492|Fantastic Steel Shirt
  - Found using: SELECT SUM(orders.quantity * items.price) as total, items.title FROM items INNER JOIN orders WHERE items.id = orders.item_id GROUP BY orders.item_id ORDER BY total DESC LIMIT 1;

11. What user spent the most?
 - User 19
 - Found using: SELECT SUM(orders.quantity * items.price) as total, orders.user_id FROM items INNER JOIN orders WHERE items.id = orders.item_id GROUP BY orders.user_id ORDER BY total DESC LIMIT 1;

12. What were the top 3 highest grossing categories?
  - 534492|Shoes, Automotive & Kids
    525240|Music, Sports & Clothing
    449496|Beauty, Toys & Sports
  - Found using: SELECT SUM(orders.quantity * items.price) as total, items.category FROM items INNER JOIN orders WHERE items.id = orders.item_id GROUP BY items.category ORDER BY total DESC LIMIT 3;

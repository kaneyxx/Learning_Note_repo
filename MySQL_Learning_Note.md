# MySQL 學習筆記
>學習是看YouTube上[Programming with Mosh](https://youtu.be/7S_tz1z_5bA)的影片  
>例子跟程式碼都是配合影片的, 建議邊看邊學習使用  
>純自己筆記用非教學文章, 如不喜請上網自行Google教學文章閱讀  

## MySQL 安裝
[官方下載頁面](https://dev.mysql.com/downloads/)
> 找適合自己的作業系統安裝, MySQL社區版可以免費下載但不得作為商業使用

### 重要事項:  
* SQL是個case insensitive的語言(**即大小寫沒有區別**), 一般會使用大寫代表指令來讓閱讀時比較舒服跟辨別, 也是養成良好的編程習慣
* 一段完整的Query指令記得以分號(;)做結尾

## 基礎指令操作
* **創建資料庫**
  > `CREATE DATABASE 資料庫名稱`
* **查看所有資料庫**
  > `SHOW DATABASES`
* **查看所有table**
  > `SHOW TABLES`
* **指定要使用的資料庫**
  > `USE 資料庫名稱`
* **註解程式碼**
  > 可以使用`--`或是`#` 從設定去調整即可
### SELECT 使用
* **選擇要提取(retrieve)的資料**
  > - `SELECT column名稱 FROM 目標表格`  
  > - 多個column用逗號分開(寫的順序關乎到提取的排列順序), 全選則用(\*)  
* **提取資料時直接去除重複內容**
  > - `SELECT DISTINCT column名稱` 出來的資料內容就會是unique的  
* **可以直接在提取資料時即作出計算**, 例如  
  > - `SELECT points+10` 提取points這column的資料後會全部加上10來呈現 
  > - 四則運算(+ - \* /), 餘數(%)都可以使用  
* **可以直接更動出來的column名稱, 一般來說retrieve的column名稱就是我們SELECT打的目標, 但是如果是在計算等等之後會變得很長, 這時候就必須賦予它新的column名**  
  > - `SELECT (point+10) AS 'discount'` 這樣出來就會叫做discount, 使用引號的目的是如果我們的名稱要取兩個字而中間是空格時不會被SQL忽略  
### WHERE 使用
* **在提取資料時增加條件(要在SELECT/FROM之後), 例如**
  > - `SELECT * FROM table_A WHERE points > 3000`  
* 比較運算子
  > - \>, >=, <, <=, =, !=(也可以寫作<>)  
* **條件後方可以加上字串, 字串要用引號包覆(單雙引號均可), 字串比大小跟排序一樣看字母**  
  > - `WHERE name = "WANG"` 
* **條件也能比對日期, MySQL的日期格式為'年-月-日'(單雙引號均可, 但不能不加)**
  > - `WHERE date > '1990-01-01'`  
  > 排序, 加上`ORDER BY 條件 升降序` 升序ASC(預設)  降序DESC  
* **利用邏輯運算子合併多個條件**  
  > - AND, OR, NOT
  > - 例如: `WHERE name != "WANG" AND points > 1000 OR birth < '1900-01-01'`  
* **IN 的使用**
  > - 先看個OR例子`WHERE state="VA" OR state="GA" OR state="MA"`  
  > - 可以用IN改寫成 `WHERE state in ("VA", "GA", "MA")`  
  > - 如果是不想要這些可以改用 NOT IN  
* **BETWEEN 的使用**  
  > - 先看個AND例子`WHERE points >= 1000 AND points <= 3000`  
  > - 可以用BETWEEN改寫成`WHERE points BETWEEN 1000 AND 3000`(BETWEEN有含首尾)  
* **LIKE 的使用**
  > - LIKE用來表示選擇條件的pattern, 就像下面的regular expression  
  > - 直接先看使用例子`WHERE last_name LIKE 'b%'`  
  > - 'b%'表示b開頭(大小寫不拘), %表示任意字元數量不拘(含0), 位置照自己的想法放也能做這樣'%b%'  
  > - '\_y' 表示y結尾, \_表示**單一**任意字元(可以多個串接)  
  > - 同樣地, 也能加上NOT來取得相反的結果  
* **REGEXP 的使用**  
  > - 這是類似於LIKE但是比較新的語法, 為regular expression的縮寫  
  > - 直接看個例子`WHERE last_name REGEXP 'field'`這個結果跟`LIKE '%field%'`一樣表示字串中任意位置含有field  
  > - 使用^來表示起始 `WHERE name REGEXP '^J'` 同LIKE的'J%'  
  > - 使用\$來表示結尾 `WHERE name REGEXP 'J$'` 同LIKE的'%J'  
  > - 使用|來聯集(OR)多個條件 `WHERE name REGEXP 'field|mac'` 表示字串中含有field或是mac者都可以,記得別留空格,會被判定進去  
  > - 使用[ ]來結合多種條件 `WHERE name REGEXP '[gim]e'` 表示字串中含有ge,ie,me的都可以, [ ]內也可以使用範圍選取, 例如[a-m],[1-9]這些  
* **IS NULL 的使用**
  > - 直接看例子`WHERE phone IS NULL` 就是提取phone這格內無值的對象  
  > - 同樣可以跟NOT搭配`IS NOT NULL`  
* **全體運算子的邏輯順序:**
  > - 括號-->乘/除/餘-->比較運算子-->加/減-->NOT-->AND-->OR/IN/BETWEEN/LIKE/REGEXP/IS NULL  
### ORDER BY 使用  
> 正常我們SELECT/FROM提取資料後, 會發現它會依照某個ID去做排序, 那個便是當初table創建丟入資料時設定的primary key, 它們往往是唯一而不重複的, 方便之後做不同table的操作跟辨識, 我們也能使用ORDER BY指令來使資料依照我們希望的方式做排序.
* **ORDER BY**
  > - 直接看例子`SELECT * FROM table ORDER BY first_name`
* **ASC/DESC 的使用**
  > - 不加的話預設值為升序
  > - ASC(ascending升序)/DESC(descending降序)加在ORDER BY的最後方
  > - `ORDER BY state DESC`
* **多個並用**
  > - `ORDER BY first_name DESC, state ASC` 依照語句順序去做排序
* 藉由創建的alias來排序
  > - `SELECT first_name, 10 AS points FROM table ORDER BY points, first_name`在這個例子中, points並不存在我們的table中, 所以他會創建一個名為points的column其中值都是10.
* **用數字代表要排序的順序**
  > - `SELECT first_name, last_name FROM table ORDER BY 1, 2` 這個1,2就是代表我們SELECT的第一個和第二個(但這方式不是很建議)
### LIMIT 使用  
> 有時候我們並沒有希望取得所有資料(特別是資料量非常龐大時), 僅取一些便能讓我們一窺全貌
* **LIMIT 的使用**
  > - `SELECT * FROM table LIMIT 3` 如此就會僅取前3個row的資料, 如果數字超過資料的row數自然就是全顯示
  > - `LIMIT 6, 3` 第一個數字表示跳過6個row不管, 然後選3個row
* 要記得LIMIT使用位置在ORDER BY後面(SELECT FROM/WHERE/ORDER BY/LIMIT)

### JOIN 使用
* **INNER JOIN**  
我們一般資料庫中有許多的tables, 分別存放著不同種類的資料, 然而我們有時候想在不同的table中抽取資料, 這時候就需要用到JOIN(有分INNER跟OUTER, 沒寫就是預設INNER)
  > - `SELECT * FROM orders JOIN customers ON orders.customer_id = customers.customer_id`  
  > - 意思是將orders與customers這倆table做inner join, 結合的條件在於使用兩表格的customer_id做配對  
  > - 看抽取出來結合的表格結果可以發現, 左邊先顯示的是orders(因為我們寫在前面)  
  > - 當然我們可以將SELECT * 改成我們想要的內容, 然而如果改的內容中有customer_id就會報錯誤, 因為SQL不知道我們到底是要抽取哪個table的customer_id, 只要註明清楚就能抽取(這情況在抽取的column存在多個表格是都是必須註明清楚的)  

小技巧:在我們寫多個table的JOIN時往往table名稱要不斷地重複寫, 所以可以改縮寫如下
```SQL=
SELECT *
FROM orders o
JOIN customers c 
    ON o.customer_id = c.customer_id
```
直接空格加上要縮寫的字串即可(**但要注意, 一旦縮寫整句query都要用縮寫**)
  
* **JOIN across databases**  
除了很多tables之外, 我們還有很多不同的databases, 如果要跨資料庫做結合呢? 例如我們想要結合sql_store下的order_items以及sql_inventory下的products  

  > - `SELECT * FROM order_items oi JOIN sql_inventory.products p ON oi.product_id = p.product_id`  
  > - 要注意的是這邊我們沒有指明order_items的資料庫是因為前面已經使用了USE指令, 目前在sql_store資料庫下, 如果在其他資料庫則必須修正不然會報錯誤  

* **self JOIN**  
表格也能"自己跟自己結合", 例如雇員中的資料有reports to代表他的上級, 有一位雇員沒有資料(null, 同時表示他是最大的), 這時候可以使用self JOIN的方式去做出一張新表格, 為所有雇員以及其回報的上級資料在後方  
  > - `SELECT * FROM employees e JOIN employees m ON e.reports_to = m.employee_id`
  > - 要注意的是雖然表格是同一個, 但是必須使用縮寫來使其能執行, 都用employees或是同一個縮寫都會報錯誤
  > - 有趣的是, 我們觀察結合後的表格可以發現, 前半段的first_name顯示的是我們具有上司回報的雇員名單, 而後半段的first_name全是上司名稱, 雖然是用同一份表格結合出來的, 但因為我們賦予了他們不同的縮寫, 所以現在**SQL會把他們'視作'不同個體**. 因此如果我們將請求改成`
SELECT e.first_name, m.first_name
FROM employees e
JOIN employees m
ON e.reports_to = m.employee_id
`會發現出來的兩行first_name分別是雇員跟回報上司的名稱!  

* **multiple tables JOIN**
接下來如果我們希望結合多個表格呢?  
  > - `SELECT * FROM orders o JOIN customers c ON o.customer_id = c.customer_id JOIN order_statuses os ON o.status = os.order_status_id`  
  > - 其實作法上就是再寫JOIN請求而已, 即A JOIN B ON A=B條件 JOIN C ON A=C條件  
  > - 多table選擇項目時要明確, 如果有某table是全部都要的可以用c.*這樣的寫法  
* **compund condition JOIN**
如果我們在結合的條件上想要多個選擇時  
  > - `SELECT * FROM order_items oi JOIN order_item_notes oin ON oi.order_id = oin.order_id AND oi.product_id = oin.product_id`  
  > - 即是在JOIN ON 條件後再用AND去串接其他條件, 非常直觀  
* **implicit JOIN syntax**
一種特殊的JOIN語法  
  > - 原先我們的`SELECT * FROM orders o JOIN customers c ON o.customer_id = c.customer_id`(explicit JOIN syntax)  
  > - 可以改寫成`SELECT * FROM orders o, customers c WHERE o.customer_id = c.customer_id`(implicit JOIN syntax)  
  > - 然而**並不建議這麼做**, 特別是當你忘記使用WHERE給予條件時, 就會出現cross join的狀況, 就是在沒條件之下, 選取的table中的每一項都互相去做配對  
* **OUTER JOIN**  
前面有提過SQL的JOIN有分inner/outer, 沒特別寫就是預設inner, 現在來看一下這倆的區別  
  > - 先看學過的inner`SELECT c.customer_id, c.first_name, o.order_id FROM customers c JOIN orders o ON c.customer_id = o.customer_id ORDER BY c.customer_id`  
  > - 可以看到出來的結果中, 有些customer_id出現兩次, 而有些則沒出現, 這是因為這些客戶並沒有下訂單, 所以資料庫中他們那欄位的資料是null, 在結合時便不會被取用; 然而如果我們想把他們也一起展示出來呢?  
  > - 這時候就能使用OUTER JOIN, 而OUTER JOIN又分作LEFT JOIN/RIGHT JOIN  
  > - 使用LEFT JOIN, 表示回傳所有左邊table的選取內容(即我們先選的那個table), 不論結合條件是否成立  
  > - 反之RIGHT JOIN就是回傳所有右邊的table內容, 不論結合條件是否成立  
* **multiple tables OUTER JOIN**  
做法與INNER JOIN一樣
* **self OUTER JOIN**  
做法與INNER JOIN一樣
* **USING 語法使用**  
在我們JOIN的條件中, 如果兩個table所使用的column名稱一樣的時候可以用USING改寫
  > - 原本是`SELECT * FROM orders o JOIN customers c ON o.customer_id = c.customer_id`  
  > - 可以改寫成`SELECT * FROM orders o JOIN customers c USING (customer_id)`  
  > - 類似地, 如果我們要多條件使用就寫成`USING (columnA, columnB)`謹記要結合的column名稱必須一模一樣  
* **NATURAL JOIN**
在MySQL中有更簡潔的結合寫法, 叫做NATURAL JOIN但是一般不建議這麼寫, 因為如果不熟悉容易發生難以預料的結果
  > - `SELECT o.order_id c.first_name FROM orders o NATURAL JOIN customers c`
  > 這結合上並無法預期他會用哪個column name去作, 所以結果才能以預料(不建議用!)
* **CROSS JOIN**
在上面段落有提到過cross join的發生, 然而如果我們想要使用CROSS JOIN是可以的, 他便是將結合的表格所有的資料都作配對, 也因此並不需要指定結合條件  
  > - 範例`SELECT c.first_name AS customer, p.name AS product FROM customer c CROSS JOIN products p`  

### UNION 使用
>上面我們基本cover了JOIN的各個層面, 見到了各種使用column去作結合的方式, 接著我們來看看如何從row的角度去作資料的提取跟結合, 我們先看一個例子, 我們希望今年(2019)的資料label上active而之前年份的資料label上archived(要注意這樣是**一個query**,中間不要分號)
```SQL=
SELECT 
    order_id,
    order_date,
    'Active' AS Status  # 這樣就label上去了
FROM orders
WHERE order_date >= '2019-01-01'
UNION  # UNION用在這裡
SELECT 
    order_id,
    order_date,
    'Archived' AS Status
FROM orders
WHERE order_date < '2019-01-01'
```
這是單一table中的選取, 也能作不同table的變化, 例如
```SQL=
SELECT first_name
FROM customers
UNION
SELECT name
FROM products
```
回傳的column會是由選取的這倆個column的資料結合而成, 而column名則是由query前者所訂(此例中即為first_name)

## INSERT/UPDATE/DELETE data
>在講述這些功能之前我們必須要知道column的各種屬性, 我們在打開table的design mode後能看到:
  - INT 整數
  - VARCHAR(50) 表示能儲存最多50字元, 不足者不補(儲存字串使用這個)
  - CHAR(50) 表示能儲存最多50字元, 不足者SQL自動補滿(不推薦)
  - DATE 日期
  - PK = PRIMARY KEY
  - AI = AUTO INCREMENT 一般是配合PRIMARY KEY使用
  - NN = NOT NULL 如果勾選即表示這column不接受NULL值
  - Default/Expression = 預設值, 如果沒有輸入值的話就會以預設值存入  
  (其他更多會在後面提到)  

### INSERT 使用
* **插入一row資料的寫法**
```SQL=
INSERT INTO table名稱
VALUE (DEFAULT, first_name, last_name, ....)
```
  > - DEFAULT位置一般是指PRIMARY KEY, 當然我們也能自己寫值, 然而用default最簡單, 因為有AUTO INCREMENT設定能讓他自動加值, 其他有預設值的欄位也能這樣寫或是鍵入它的預設值均可
  > - 其他部分就按照目的table的column來鍵入
  > - 我們也能不照順序, 而是自己的順序鍵入(順序隨我們), 例如：
```SQL=
INSERT INTO customers (
    first_name,
    last_name,
    birth_date,
    address)
VALUE (
    John,
    Smith,
    '1990-01-01',
    'address')
```
* **插入多個rows的寫法**
```SQL=
INSERT INTO table名稱
VALUES (要鍵入的row1),
       (要鍵入的row2),
       (要鍵入的row3)
```

* **Inserting Hierarchical Rows**
有些table間的關係並非是一對一的, 例如我們看orders可以看到各個訂單的id, 人名, 日期...但詳細的商品內容則是在order_items; 而order_items中可以看到一個訂單下不只有一個prodoct, 因此orders一個row對應過去很可能不只有對應到order_items的一個row, 這便是"親子代"的關係, 我們可將orders視為親代, 而order_items視為子代. 在這樣的關聯性下, 我們會希望我們insert資料時候能夠同步的去新增相關的內容
  > - `SELECT LAST_INSERT_ID()`這函數能顯示最後插入的ID, 使用這個函數可以讓我們在親代表格插入資料後, 順利地去進行子代的插入. 完整大致如下
```SQL=1
INSERT INTO orders (customer_id, order_date, status)
VALUES (1, '2019-01-01', 1);  # parent table insert完成

INSERT INTO order_items    # child table insert
VALUES 
    (LAST_INSERT_ID(), 1, 1, 2.95),
    (LAST_INSERT_ID(), 2, 1, 3.95)

```
* **copy a table from existed one**  
有時候在資料很大時, 我們要進行資料的遷移上不可能一個個去insert, 因此直接copy整個table就是用在這個時候.
```
CREATE TABLE 要建立的table名稱 AS 
SELECT * FROM 要被copy的table名稱  # subquery
```
  > - 要特別注意的是, 這樣copy而形成的表格, 會失去primary key跟auto increment的特性!
  > - 另外上面那樣的寫法中, SELECT那行稱之為subquery, 因此我們能用不同的subquery來進行特徵化的插入  
```SQL=
INSERT INTO orders_archived 
SELECT *  # 從這開始為subquery
FROM orders
WHERE order_date < '2019-01-01'
```
### UPDATE 使用  
當我們需要進行資料更動(更新)時使用, 基本語法為
```
UPDATE table
SET column(s) = values
WHERE condition
```
* **Update multiple rows**

  > - 當我們想要進行多個row的資料更新時, 語法是一模一樣的僅僅是在WHERE的條件設定上有所更動, 然而在練習時使用的MySQL workbench中設定內有個safe UPDATE 及 delete的限制, 禁止一次進行多row的更動以避免悲劇. 如果今天我們是在別的程序中呼叫或是進行query則沒有這種限制.(可以在workbench的設定中更改)

* 使用subquery技巧來進行update
  > - subquery的併用是非常實用的方法, 讓我們設想一下, 我們在進行invoices的資料更新時, 關於客戶辨識上僅有client_id, 然而我們客戶傳送過來的是名字, 我們要如何使用name來獲取client_id進而update我們的invoices
```SQL=
UPDATE invoices
SET payment_date = due_date
WHERE client_id =  # 一般在這加上條件目標的client_id
        (SELECT client_id 
        FROM clients
        WHERE name = 'Myworks')
```
而當我們的WHERE條件目標不在是單一時則不能再使用'='而要用IN, 如下
```SQL=
UPDATE invoices
SET payment_date = due_date
WHERE client_id IN
        (SELECT client_id 
        FROM clients
        WHERE state IN ('CA', 'NY'))
```

### DELETE 使用
前面已經學習過SELECT/INSERT/UPDATE等等用法, 接著我們來學習如何刪除資料, 直接看語法
```SQL=
DELETE FROM table
WHERE 條件(也可以接subquery)
```
範例:
```SQL=
DELETE FROM invoices
WHERE client_id =
( SELECT *
  FROM clients
  WHERE name = 'Myworks')
```
* 繼續接下去的課程之前, 因為我們對於課程使用的資料庫進行過許多的更動, 所以我們要做還原的動作(其實就是重新建出原本使用的資料庫), 我們回到最早課程使用的script執行它即可.

---
之後就是收費課程, 有興趣者可以自己去影片作者網站購買繼續學習~(非業配)
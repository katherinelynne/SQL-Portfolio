---


---

<h1 id="adventure-works-cycles"><span class="prefix"></span><span class="content">Adventure Works Cycles</span><span class="suffix"></span></h1>
<blockquote>
<p>AdventureWorks database supports standard online transaction processing scenarios for a fictitious bicycle manufacture names Adventure Works Cycles.</p>
</blockquote>
<p><img src="https://i.ibb.co/3NZ80Cw/image-2022-02-26-223757.png" alt="enter image description here"></p>
<h2 id="tables-used"><span class="prefix"></span><span class="content">Tables used</span><span class="suffix"></span></h2>
<p>Tables used to answer case study questions:</p>
<p>Sales.SalesOrderHeader<br>
Sales.SalesOrderDetail<br>
Production.Product<br>
Sales.SpecialOffer<br>
Sales.SpecialOfferProduct<br>
Sales.SalesPersonQuotaHistory<br>
Sales.SalesPerson<br>
HumanResources.EmployeeDepartmentHistory</p>
<h2 id="case-study-questions"><span class="prefix"></span><span class="content">Case Study Questions:</span><span class="suffix"></span></h2>
<p><strong>Sales/Customer Analysis</strong></p>
<ol>
<li>What is the total amount each customer spent ?</li>
<li>How many times has each customer made a purchase?</li>
<li>What is the most purchased product and how many times was it purchased? (Most popular product).</li>
<li>What are the top 5 products with a SpecialOfferID?</li>
</ol>
<blockquote>
<p><em><strong>Purpose</strong></em>: We will discover a deeper understanding of customer spending and what the most popular products are in order to determine what steps could be taken to increase the customer base and increase sales.</p>
</blockquote>
<p><strong>Sales Person Analysis</strong></p>
<ol>
<li>Which sales person has the highest sales quota?</li>
<li>Which sales person had the highest sales increase from SalesLastYear versus SalesYTD?</li>
<li>Is there any correlation between the top 5 best sales persons and their amount of time at the BuisnessEntity?</li>
</ol>
<blockquote>
<p><em><strong>Purpose</strong></em>: We are determining if tenure/business knowledge is a potential reason for a successful salesperson.</p>
</blockquote>
<p><strong>Product Analysis</strong><br>
5. What is the cost per product (including suppliers, production, freight)?</p>
<blockquote>
<p><em><strong>Purpose</strong></em>:</p>
</blockquote>
<h2 id="solutions"><span class="prefix"></span><span class="content">Solutions:</span><span class="suffix"></span></h2>
<p><strong>1. What is the total amount each customer spent ?</strong><br>
The first thing I did was investigate the sales.SalesOrderDetail table which includes the sales order ID, the unit price, and whether or not the product had a “special offer” which I’m assuming that is a sale of the item considering that the unit price is slightly higher when comparing to the Product ID table.</p>
<p>I notice that there is no total amount spent column so I investigated the  sales.SalesOrderHeader table to find that there is a subtotal column that is the amount that the customer spent in total. But instead of joining the subtotal column onto the SalesOrderDetail table, I created a temp table to calculate the subtotal for each sales order ID. Note, this subtotal that the customer spent does not include taxes or shipping. To get the total amount spent on only products, I will not be including taxes or shipping in my query.</p>
<p>I created a temp table called <strong>customertotalspent</strong>.</p>
<p>Then I selected the column names that I wanted to keep from the SamesOrderDetail table first then I selected the 2 column names I wanted to keep from the SalesOrderHeader table. I created a new column named LineTotal to calculate the amount of each product a customer bought from each sales order ID. The calculation came from AdentureWorks attributes table where I copied and pasted into my query.</p>
<p>Next, I left joined SalesOrderHeard onto SalesOrderDetail table and giving each table an alias.</p>
<pre class=" language-sql"><code class="prism  language-sql"><span class="token operator">*</span> this <span class="token operator">is</span> note<span class="token punctuation">;</span>
<span class="token keyword">drop</span> <span class="token keyword">table</span> customertotalspent<span class="token punctuation">;</span>

<span class="token keyword">create</span> <span class="token keyword">temp</span> <span class="token keyword">table</span> customertotalspent <span class="token keyword">as</span>
<span class="token keyword">select</span> 
	<span class="token number">a</span><span class="token punctuation">.</span>salesorderid<span class="token punctuation">,</span>
	<span class="token number">a</span><span class="token punctuation">.</span>salesorderdetailid<span class="token punctuation">,</span>
	<span class="token number">a</span><span class="token punctuation">.</span>orderqty<span class="token punctuation">,</span>
	<span class="token number">a</span><span class="token punctuation">.</span>productid<span class="token punctuation">,</span>
	<span class="token number">a</span><span class="token punctuation">.</span>unitprice<span class="token punctuation">,</span>
	<span class="token number">a</span><span class="token punctuation">.</span>unitpricediscount<span class="token punctuation">,</span>
	<span class="token number">b</span><span class="token punctuation">.</span>customerid<span class="token punctuation">,</span>
	<span class="token number">b</span><span class="token punctuation">.</span>orderdate<span class="token punctuation">,</span>
	<span class="token number">a</span><span class="token punctuation">.</span>UnitPrice <span class="token operator">*</span> <span class="token punctuation">(</span><span class="token number">1</span> <span class="token operator">-</span> <span class="token number">a</span><span class="token punctuation">.</span>UnitPriceDiscount<span class="token punctuation">)</span> <span class="token operator">*</span> <span class="token number">a</span><span class="token punctuation">.</span>OrderQty <span class="token keyword">as</span> LineTotal
<span class="token keyword">from</span> sales<span class="token punctuation">.</span>salesorderdetail <span class="token number">a</span>
<span class="token keyword">left</span> <span class="token keyword">join</span> sales<span class="token punctuation">.</span>salesorderheader <span class="token number">b</span>
<span class="token keyword">on</span> <span class="token number">a</span><span class="token punctuation">.</span>salesorderid<span class="token operator">=</span><span class="token number">b</span><span class="token punctuation">.</span>salesorderid
<span class="token punctuation">;</span>
</code></pre>
<p>Now I can query from this table to show the total subtotal amount each customer spent.</p>
<pre class=" language-sql"><code class="prism  language-sql"><span class="token keyword">select</span>
	customerid<span class="token punctuation">,</span>
	<span class="token function">sum</span><span class="token punctuation">(</span>linetotal<span class="token punctuation">)</span> <span class="token keyword">as</span> totalspent
<span class="token keyword">from</span> customertotalspent
<span class="token keyword">group</span> <span class="token keyword">by</span> customerid
<span class="token keyword">order</span> <span class="token keyword">by</span> totalspent <span class="token keyword">desc</span><span class="token punctuation">;</span>
</code></pre>
<p><img src="https://i.ibb.co/23CHtt1/image-2022-02-26-125608.png" alt="enter image description here"></p>
<p><strong>2. How many times has each customer made a purchase?</strong><br>
Here, I simply grouped by CustomerID and counted the distinct SalesOrderID for each to get the amount of times each customer has made a purchase. Using the table I previously created,  <strong>customertotalspent</strong>, I was able to easily pull the CustomerID information against the SalesOrderID that associated with each. There are a total of 19, 119 customers.</p>
<pre class=" language-sql"><code class="prism  language-sql"><span class="token keyword">select</span> 
	customerid<span class="token punctuation">,</span>
	<span class="token function">count</span><span class="token punctuation">(</span><span class="token keyword">distinct</span> salesorderid<span class="token punctuation">)</span> <span class="token keyword">as</span> totaltimespurchased
	<span class="token keyword">from</span> customertotalspent
<span class="token keyword">group</span> <span class="token keyword">by</span> customerid
<span class="token keyword">order</span> <span class="token keyword">by</span> totaltimespurchased <span class="token keyword">desc</span><span class="token punctuation">;</span>
</code></pre>
<p><img src="https://i.ibb.co/r4rz8RY/image-2022-02-26-221443.png" alt="enter image description here"></p>
<p><strong>3. What are the top 5 products and how many times were each purchased? (Most popular products).</strong></p>
<pre class=" language-sql"><code class="prism  language-sql"><span class="token keyword">select</span> 
	<span class="token number">b</span><span class="token punctuation">.</span>name <span class="token keyword">as</span> product_name<span class="token punctuation">,</span>
	<span class="token function">sum</span><span class="token punctuation">(</span><span class="token number">a</span><span class="token punctuation">.</span>orderqty<span class="token punctuation">)</span> <span class="token keyword">as</span> top_purchased_product<span class="token punctuation">,</span>
	<span class="token number">a</span><span class="token punctuation">.</span>productid <span class="token keyword">as</span> product_id
<span class="token keyword">from</span> sales<span class="token punctuation">.</span>salesorderdetail <span class="token number">a</span>
<span class="token keyword">left</span> <span class="token keyword">join</span> production<span class="token punctuation">.</span>product <span class="token number">b</span>
<span class="token keyword">on</span> <span class="token number">a</span><span class="token punctuation">.</span>productid<span class="token operator">=</span><span class="token number">b</span><span class="token punctuation">.</span>productid
<span class="token keyword">group</span> <span class="token keyword">by</span> <span class="token number">a</span><span class="token punctuation">.</span>productid<span class="token punctuation">,</span> <span class="token number">b</span><span class="token punctuation">.</span>name
<span class="token keyword">order</span> <span class="token keyword">by</span> top_purchased_product <span class="token keyword">desc</span>
<span class="token keyword">limit</span> <span class="token number">5</span><span class="token punctuation">;</span>
</code></pre>
<p><img src="https://i.ibb.co/xmkrtsL/image-2022-03-03-220600.png" alt="enter image description here"></p>
<p>I cant believe a water bottle is the most purchased product…</p>
<p><strong>4. What are the top 5 products with a SpecialOfferID?</strong></p>
<pre class=" language-sql"><code class="prism  language-sql"><span class="token keyword">select</span> 
	<span class="token number">b</span><span class="token punctuation">.</span>name <span class="token keyword">as</span> product_name<span class="token punctuation">,</span>	
	<span class="token function">sum</span><span class="token punctuation">(</span><span class="token number">a</span><span class="token punctuation">.</span>orderqty<span class="token punctuation">)</span> <span class="token keyword">as</span> top_purchased_product<span class="token punctuation">,</span>
	<span class="token number">b</span><span class="token punctuation">.</span>productid
<span class="token keyword">from</span> sales<span class="token punctuation">.</span>salesorderdetail <span class="token number">a</span>
<span class="token keyword">left</span> <span class="token keyword">join</span> production<span class="token punctuation">.</span>product <span class="token number">b</span>
	<span class="token keyword">on</span> <span class="token number">a</span><span class="token punctuation">.</span>productid<span class="token operator">=</span><span class="token number">b</span><span class="token punctuation">.</span>productid
<span class="token keyword">left</span> <span class="token keyword">join</span> sales<span class="token punctuation">.</span>specialofferproduct <span class="token number">c</span>
	<span class="token keyword">on</span> <span class="token number">b</span><span class="token punctuation">.</span>productid<span class="token operator">=</span><span class="token number">c</span><span class="token punctuation">.</span>productid
	<span class="token operator">and</span> <span class="token number">c</span><span class="token punctuation">.</span>specialofferid <span class="token operator">&lt;&gt;</span> <span class="token number">1</span>
<span class="token keyword">group</span> <span class="token keyword">by</span>  <span class="token number">b</span><span class="token punctuation">.</span>name<span class="token punctuation">,</span> <span class="token number">b</span><span class="token punctuation">.</span>productid
<span class="token keyword">order</span> <span class="token keyword">by</span> top_purchased_product <span class="token keyword">desc</span>
<span class="token keyword">limit</span> <span class="token number">12</span><span class="token punctuation">;</span>
<span class="token punctuation">`</span><span class="token punctuation">`</span><span class="token punctuation">`</span>it<span class="token operator">+</span><span class="token punctuation">]</span><span class="token punctuation">(</span>https:<span class="token comment">//stackedit.net/).</span>
</code></pre>


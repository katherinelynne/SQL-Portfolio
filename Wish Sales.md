---


---

<p><strong>1.   Does the rating of a seller matter ?</strong> When the price of a product is sufficiently low, some buyers may tend to not care that much about the rating of a seller since the cost is low enough. One may want to try to see if there is a breakpoint regarding the price and that tendency. Or one might want to play around this question.</p>
<p>Which merchants sold the most product? And of those 10 merchants, what are each of their average ratings?</p>
<pre class=" language-sql"><code class="prism  language-sql"><span class="token keyword">SELECT</span> MERCHANT_TITLE<span class="token punctuation">,</span>
	<span class="token function">SUM</span><span class="token punctuation">(</span>UNITS_SOLD<span class="token punctuation">)</span> <span class="token keyword">AS</span> TOTAL_SALES<span class="token punctuation">,</span>
	<span class="token function">ROUND</span><span class="token punctuation">(</span><span class="token function">AVG</span><span class="token punctuation">(</span>MERCHANT_RATING<span class="token punctuation">)</span><span class="token punctuation">,</span> <span class="token number">2</span><span class="token punctuation">)</span>
	<span class="token keyword">AS</span> MEAN_RATING
<span class="token keyword">FROM</span> SUMMERPRODUCTS<span class="token punctuation">.</span>SP
<span class="token keyword">GROUP</span> <span class="token keyword">BY</span> MERCHANT_TITLE<span class="token punctuation">,</span> MERCHANT_RATING
<span class="token keyword">ORDER</span> <span class="token keyword">BY</span> TOTAL_SALES <span class="token keyword">DESC</span><span class="token punctuation">,</span> MEAN_RATING <span class="token keyword">DESC</span>
<span class="token keyword">LIMIT</span> <span class="token number">10</span><span class="token punctuation">;</span>
</code></pre>
<p><img src="https://i.ibb.co/rH1qyq7/image-2021-12-28-214648.png" alt="enter image description here"></p>
<p>How many units did the top 10 highest rated merchants sell?</p>
<pre class=" language-sql"><code class="prism  language-sql"><span class="token keyword">SELECT</span>
	MERCHANT_TITLE<span class="token punctuation">,</span>
	<span class="token function">SUM</span><span class="token punctuation">(</span>UNITS_SOLD<span class="token punctuation">)</span> <span class="token keyword">AS</span> TOTAL_SALES<span class="token punctuation">,</span>
	<span class="token function">ROUND</span><span class="token punctuation">(</span><span class="token function">AVG</span><span class="token punctuation">(</span>MERCHANT_RATING<span class="token punctuation">)</span><span class="token punctuation">,</span>
		<span class="token number">2</span><span class="token punctuation">)</span> <span class="token keyword">AS</span> MEAN_RATING
<span class="token keyword">FROM</span> SUMMERPRODUCTS<span class="token punctuation">.</span>SP
<span class="token keyword">GROUP</span> <span class="token keyword">BY</span> 
	MERCHANT_TITLE<span class="token punctuation">,</span>
	MERCHANT_RATING
<span class="token keyword">ORDER</span> <span class="token keyword">BY</span> MEAN_RATING <span class="token keyword">DESC</span>
<span class="token keyword">LIMIT</span> <span class="token number">10</span><span class="token punctuation">;</span>
</code></pre>
<p><img src="https://i.ibb.co/wRRBSMs/image-2021-12-28-193914.png" alt="enter image description here"></p>
<p>None of the highest rated sellers have the top 10 highest rating. The top rated sellers have not sold nearly as many units as merchants who sold the most product. However, all of the merchants who have the highest units sold also have ratings at least 3.8 and above (which is pretty good).</p>
<p>Do any of these “top merchants” have the most popular product?</p>
<p><img src="https://i.ibb.co/WVBb39G/image-2021-12-28-222327.png" alt="enter image description here"></p>
<p><em>The correlation coefficient between the merchants overall rating and the units of product sold. Here, we see a weak positive correlation.</em></p>
<pre class=" language-sql"><code class="prism  language-sql"><span class="token keyword">SELECT</span> CORR<span class="token punctuation">(</span>MERCHANT_RATING<span class="token punctuation">,</span>

								UNITS_SOLD<span class="token punctuation">)</span> CORRELATION_COEFFICIENT
<span class="token keyword">FROM</span> SUMMERPRODUCTS<span class="token punctuation">.</span>SALES<span class="token punctuation">;</span>
</code></pre>
<p><img src="https://i.ibb.co/fv2Lbq2/image-2022-01-02-231121.png" alt="enter image description here"></p>
<p><em>Yes</em>, it appears that the top 5 most popular products are <em>from</em> the top 5 merchants with the highest overall sales. Merchants who have the highest overall rating have some of the lowest sales.</p>
<p><em>The correlation coefficient between the merchants overall rating and number of ratings from a customer. Here, we see a weak positive correlation.</em></p>
<pre class=" language-sql"><code class="prism  language-sql"><span class="token keyword">SELECT</span> CORR<span class="token punctuation">(</span>MERCHANT_RATING_COUNT<span class="token punctuation">,</span>

								MERCHANT_RATING<span class="token punctuation">)</span> MERCHANT_CORRELATION
<span class="token keyword">FROM</span> SUMMERPRODUCTS<span class="token punctuation">.</span>SALES<span class="token punctuation">;</span>
</code></pre>
<p><img src="https://i.ibb.co/zGWB3cM/image-2022-01-02-232546.png" alt="enter image description here"></p>
<p><strong>2. Is there a correlation between ratings and sales?</strong><br>
Correlation is another method of  <em>sales forecasting</em>. Correlation looks at the  <strong>strength of a relationship</strong> between two variables.</p>
<p>Correlation is usually measured by using a scatter diagram, on which data points are plotted. For example, a data point might measure the number of customer enquiries that are generated per week (x-axis) against the amount spent on advertising (y-axis).</p>
<p>The sign of the correlation coefficient indicates the direction of the relationship. … For this kind of data, we would consider <strong>correlations above 0.4 to be relatively strong</strong>; correlations between 0.2 and 0.4 are moderate, and those below 0.2 are considered weak.</p>
<p><em>Below is the correlation coefficient between the product rating and the amount of product sold. Here, we see the correlation between the products rating and the amount of product sold is statistically significant.</em></p>
<pre class=" language-sql"><code class="prism  language-sql"><span class="token keyword">SELECT</span> CORR<span class="token punctuation">(</span>RATING<span class="token punctuation">,</span>

								UNITS_SOLD<span class="token punctuation">)</span> PRODUCT_RATING_CORRELATION
<span class="token keyword">FROM</span> SUMMERPRODUCTS<span class="token punctuation">.</span>SALES<span class="token punctuation">;</span>
</code></pre>
<p><img src="https://i.ibb.co/D5fY6n9/image-2022-01-02-233545.png" alt="enter image description here"></p>
<p><strong>3. Do ads have an impact on sales?</strong><br>
For marketing, it might be useful to know that there is a predictable relationship between sales and advertising.</p>
<p>The top 10 products where merchants uses ads</p>
<pre class=" language-sql"><code class="prism  language-sql"><span class="token keyword">SELECT</span> TITLE<span class="token punctuation">,</span>
	UNITS_SOLD<span class="token punctuation">,</span>
	USES_AD_BOOSTS
<span class="token keyword">FROM</span> SUMMERPRODUCTS<span class="token punctuation">.</span>SALES
<span class="token keyword">GROUP</span> <span class="token keyword">BY</span> TITLE<span class="token punctuation">,</span>
	UNITS_SOLD<span class="token punctuation">,</span>
	USES_AD_BOOSTS
<span class="token keyword">ORDER</span> <span class="token keyword">BY</span> USES_AD_BOOSTS <span class="token keyword">DESC</span><span class="token punctuation">,</span>
	UNITS_SOLD <span class="token keyword">DESC</span><span class="token punctuation">;</span>
</code></pre>
<p><img src="https://i.ibb.co/KVjnRx1/image-2022-01-03-211017.png" alt="enter image description here"></p>
<p>The top 10 products where merchants did not use ads</p>
<pre class=" language-sql"><code class="prism  language-sql"><span class="token keyword">SELECT</span> TITLE<span class="token punctuation">,</span>
	UNITS_SOLD<span class="token punctuation">,</span>
	USES_AD_BOOSTS
<span class="token keyword">FROM</span> SUMMERPRODUCTS<span class="token punctuation">.</span>SALES
<span class="token keyword">GROUP</span> <span class="token keyword">BY</span> TITLE<span class="token punctuation">,</span>
	UNITS_SOLD<span class="token punctuation">,</span>
	USES_AD_BOOSTS
<span class="token keyword">ORDER</span> <span class="token keyword">BY</span> USES_AD_BOOSTS<span class="token punctuation">,</span>
	UNITS_SOLD <span class="token keyword">DESC</span><span class="token punctuation">;</span>
</code></pre>
<p><img src="https://i.ibb.co/gyqdS7T/image-2022-01-03-212010.png" alt="enter image description here"></p>
<p>Code written by <a href="https://www.linkedin.com/in/hicham-gougane-56367b134/?lipi=urn:li:page:messaging_thread;1b89de64-81cf-4fc6-812b-881b468833d0&amp;licu=urn:li:control:d_flagship3_messaging-view_profile">Hicham</a><br>
<em>“There’s a little improvement on sales when we compare it based on how many times the seller paid for ads… we notice that max items sold is same even they used ads less often”</em></p>
<pre class=" language-sql"><code class="prism  language-sql"><span class="token keyword">SELECT</span> 
<span class="token keyword">DISTINCT</span> uses_ad_boosts<span class="token punctuation">,</span>
<span class="token function">AVG</span><span class="token punctuation">(</span>units_sold<span class="token punctuation">)</span> <span class="token keyword">AS</span> Average_units_sold<span class="token punctuation">,</span>
<span class="token function">MAX</span><span class="token punctuation">(</span>units_sold<span class="token punctuation">)</span> <span class="token keyword">AS</span> Max_units_sold<span class="token punctuation">,</span>
<span class="token function">MIN</span><span class="token punctuation">(</span>units_sold<span class="token punctuation">)</span> <span class="token keyword">AS</span> Min_units_sold<span class="token punctuation">,</span>
<span class="token punctuation">(</span><span class="token keyword">SELECT</span> 
<span class="token function">COUNT</span><span class="token punctuation">(</span>uses_ad_boosts<span class="token punctuation">)</span> 
<span class="token keyword">FROM</span> summerproducts<span class="token punctuation">.</span>sales
<span class="token keyword">WHERE</span> uses_ad_boosts <span class="token operator">=</span> <span class="token number">0</span><span class="token punctuation">)</span> <span class="token keyword">AS</span> Count_ad_usage
<span class="token keyword">FROM</span> summerproducts<span class="token punctuation">.</span>sales
<span class="token keyword">WHERE</span> uses_ad_boosts <span class="token operator">=</span> <span class="token number">0</span>
<span class="token keyword">GROUP</span> <span class="token keyword">BY</span> uses_ad_boosts
<span class="token keyword">UNION</span>
<span class="token keyword">SELECT</span> 
<span class="token keyword">DISTINCT</span> uses_ad_boosts<span class="token punctuation">,</span>
<span class="token function">AVG</span><span class="token punctuation">(</span>units_sold<span class="token punctuation">)</span> <span class="token keyword">AS</span> Average_units_sold<span class="token punctuation">,</span>
<span class="token function">MAX</span><span class="token punctuation">(</span>units_sold<span class="token punctuation">)</span> <span class="token keyword">AS</span> Max_units_sold<span class="token punctuation">,</span>
<span class="token function">MIN</span><span class="token punctuation">(</span>units_sold<span class="token punctuation">)</span> <span class="token keyword">AS</span> Min_units_sold<span class="token punctuation">,</span>
<span class="token punctuation">(</span><span class="token keyword">SELECT</span> 
<span class="token function">COUNT</span><span class="token punctuation">(</span>uses_ad_boosts<span class="token punctuation">)</span> 
<span class="token keyword">FROM</span> summerproducts<span class="token punctuation">.</span>sales
<span class="token keyword">WHERE</span> uses_ad_boosts <span class="token operator">=</span> <span class="token number">1</span><span class="token punctuation">)</span> <span class="token keyword">AS</span> Count_ad_usage
<span class="token keyword">FROM</span> summerproducts<span class="token punctuation">.</span>sales
<span class="token keyword">WHERE</span> uses_ad_boosts <span class="token operator">=</span> <span class="token number">1</span>
<span class="token keyword">GROUP</span> <span class="token keyword">BY</span> uses_ad_boosts
</code></pre>
<p><img src="https://i.ibb.co/7th8NrH/image-2022-01-03-213227.png" alt="enter image description here"></p>
<p><strong>4. <strong>Do bad products<br>
sell? Does price factor into this?</strong></strong><br>
When comparing the product’s rating to its average amount of product sold.</p>
<pre class=" language-sql"><code class="prism  language-sql"><span class="token keyword">SELECT</span> RATING<span class="token punctuation">,</span>
	UNITS_SOLD<span class="token punctuation">,</span>
	PRICE
<span class="token keyword">FROM</span> SUMMERPRODUCTS<span class="token punctuation">.</span>SALES
<span class="token keyword">WHERE</span> RATING <span class="token operator">&lt;=</span> <span class="token number">3</span>
<span class="token keyword">GROUP</span> <span class="token keyword">BY</span> RATING<span class="token punctuation">,</span> UNITS_SOLD<span class="token punctuation">,</span> PRICE
<span class="token keyword">ORDER</span> <span class="token keyword">BY</span> UNITS_SOLD <span class="token keyword">DESC</span>
</code></pre>
<p><img src="https://i.ibb.co/8bjxvV8/image-2022-01-03-223428.png" alt="enter image description here"></p>
<p>OR<br>
Comparing the distinct rating between the average amount of product sold.</p>
<pre class=" language-sql"><code class="prism  language-sql"><span class="token keyword">SELECT</span> <span class="token keyword">DISTINCT</span><span class="token punctuation">(</span>RATING<span class="token punctuation">)</span><span class="token punctuation">,</span>
	<span class="token function">AVG</span><span class="token punctuation">(</span>UNITS_SOLD<span class="token punctuation">)</span> AVG_UNIT_SOLD<span class="token punctuation">,</span>
	<span class="token function">AVG</span><span class="token punctuation">(</span>PRICE<span class="token punctuation">)</span> AVG_PRICE
<span class="token keyword">FROM</span> SUMMERPRODUCTS<span class="token punctuation">.</span>SALES
<span class="token keyword">WHERE</span> RATING <span class="token operator">&lt;=</span> <span class="token number">3</span>
<span class="token keyword">GROUP</span> <span class="token keyword">BY</span> RATING
<span class="token keyword">ORDER</span> <span class="token keyword">BY</span> AVG_UNIT_SOLD <span class="token keyword">DESC</span>
</code></pre>
<p>Credits:<br>
<a href="https://www.tutor2u.net/business/reference/correlationkEdit+">https://www.tutor2u.net/business/reference/correlationkEdit+</a>](<a href="https://stackedit.net/">https://stackedit.net/</a>).</p>


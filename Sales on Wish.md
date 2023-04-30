---


---

<h1 id="analyze"><span class="prefix"></span><span class="content">Analyze</span><span class="suffix"></span></h1>
<p><strong>1. Identify the 50 most popular products.</strong>  Businesses want to optimize their stock, so they want to stock more of a product that will have more sales. No one would stock 10K units of a product that is only for a small niche, it would be a loss of money.</p>
<p>-Padmavathi’s code:</p>
<pre class=" language-sql"><code class="prism  language-sql"><span class="token keyword">SELECT</span> title<span class="token punctuation">,</span> <span class="token function">sum</span><span class="token punctuation">(</span>total_salevalue<span class="token punctuation">)</span> <span class="token keyword">AS</span> total_sale_val_by_amt
<span class="token keyword">FROM</span> summerproducts
<span class="token keyword">GROUP</span> <span class="token keyword">BY</span> title
<span class="token keyword">ORDER</span> <span class="token keyword">BY</span> <span class="token function">sum</span><span class="token punctuation">(</span>total_salevalue<span class="token punctuation">)</span>
<span class="token keyword">DESC</span><span class="token punctuation">;</span>
</code></pre>
<hr>
<p><strong>2.   Does the rating of a seller matter ?</strong> When the price of a product is sufficiently low, some buyers may tend to not care that much about the rating of a seller since the cost is low enough. One may want to try to see if there is a breakpoint regarding the price and that tendency. Or one might want to play around this question.</p>
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
<p><em>Yes</em>, it appears that the top 5 most popular products are <em>from</em> the top 5 merchants with the highest overall sales. But this is to be expected, the real answer lies within the ratings of these top 5 merchants.</p>
<p>Merchants who have the highest overall rating have some of the lowest sales.</p>
<hr>
<p><strong>3. Is there a correlation between ratings and sales of those 50 products? Does the price factor into this?</strong></p>
<hr>
<p><strong>4. Do ads have any impact on sales?</strong> Hicham</p>
<pre class=" language-sql"><code class="prism  language-sql"><span class="token comment">/*
Select whether the seller paid or ads for not
The value 0 means no ad boosts used
The value 1 means ad boosts used
*/</span>
<span class="token keyword">SELECT</span> <span class="token keyword">DISTINCT</span> USES_AD_BOOSTS<span class="token punctuation">,</span>
	<span class="token function">AVG</span><span class="token punctuation">(</span>UNITS_SOLD<span class="token punctuation">)</span> <span class="token keyword">AS</span> AVERAGE_UNITS_SOLD<span class="token punctuation">,</span>
	<span class="token function">MAX</span><span class="token punctuation">(</span>UNITS_SOLD<span class="token punctuation">)</span> <span class="token keyword">AS</span> MAX_UNITS_SOLD<span class="token punctuation">,</span>
	<span class="token function">MIN</span><span class="token punctuation">(</span>UNITS_SOLD<span class="token punctuation">)</span> <span class="token keyword">AS</span> MIN_UNITS_SOLD<span class="token punctuation">,</span>

	<span class="token punctuation">(</span><span class="token keyword">SELECT</span> <span class="token function">COUNT</span><span class="token punctuation">(</span>USES_AD_BOOSTS<span class="token punctuation">)</span>
		<span class="token keyword">FROM</span> <span class="token punctuation">[</span>DBO<span class="token punctuation">]</span><span class="token punctuation">.</span><span class="token punctuation">[</span><span class="token string">'SUMMER-PRODUCTS-$'</span><span class="token punctuation">]</span>
		<span class="token keyword">WHERE</span> USES_AD_BOOSTS <span class="token operator">=</span> <span class="token number">0</span><span class="token punctuation">)</span> <span class="token keyword">AS</span> COUNT_AD_USAGE
<span class="token keyword">FROM</span> <span class="token punctuation">[</span>DBO<span class="token punctuation">]</span><span class="token punctuation">.</span><span class="token punctuation">[</span><span class="token string">'SUMMER-PRODUCTS-$'</span><span class="token punctuation">]</span>
<span class="token keyword">WHERE</span> USES_AD_BOOSTS <span class="token operator">=</span> <span class="token number">0</span>
<span class="token keyword">GROUP</span> <span class="token keyword">BY</span> USES_AD_BOOSTS
<span class="token keyword">UNION</span>
<span class="token keyword">SELECT</span> <span class="token keyword">DISTINCT</span> USES_AD_BOOSTS<span class="token punctuation">,</span>
	<span class="token function">AVG</span><span class="token punctuation">(</span>UNITS_SOLD<span class="token punctuation">)</span> <span class="token keyword">AS</span> AVERAGE_UNITS_SOLD<span class="token punctuation">,</span>
	<span class="token function">MAX</span><span class="token punctuation">(</span>UNITS_SOLD<span class="token punctuation">)</span> <span class="token keyword">AS</span> MAX_UNITS_SOLD<span class="token punctuation">,</span>
	<span class="token function">MIN</span><span class="token punctuation">(</span>UNITS_SOLD<span class="token punctuation">)</span> <span class="token keyword">AS</span> MIN_UNITS_SOLD<span class="token punctuation">,</span>

	<span class="token punctuation">(</span><span class="token keyword">SELECT</span> <span class="token function">COUNT</span><span class="token punctuation">(</span>USES_AD_BOOSTS<span class="token punctuation">)</span>
		<span class="token keyword">FROM</span> <span class="token punctuation">[</span>DBO<span class="token punctuation">]</span><span class="token punctuation">.</span><span class="token punctuation">[</span><span class="token string">'SUMMER-PRODUCTS-$'</span><span class="token punctuation">]</span>
		<span class="token keyword">WHERE</span> USES_AD_BOOSTS <span class="token operator">=</span> <span class="token number">1</span><span class="token punctuation">)</span> <span class="token keyword">AS</span> COUNT_AD_USAGE
<span class="token keyword">FROM</span> <span class="token punctuation">[</span>DBO<span class="token punctuation">]</span><span class="token punctuation">.</span><span class="token punctuation">[</span><span class="token string">'SUMMER-PRODUCTS-$'</span><span class="token punctuation">]</span>
<span class="token keyword">WHERE</span> USES_AD_BOOSTS <span class="token operator">=</span> <span class="token number">1</span>
<span class="token keyword">GROUP</span> <span class="token keyword">BY</span> USES_AD_BOOSTS
</code></pre>
<p><img src="https://i.ibb.co/Gkxs4N0/image-2021-12-28-224419.png" alt="enter image description here"></p>
<p><strong>5. Does the seller’s fame factor into those 50 top products?</strong></p>


---


---

<h1 id="wine-tasting"><span class="prefix"></span><span class="content">Wine Tasting</span><span class="suffix"></span></h1>
<blockquote>
<p>This dataset was provided by Maven Analytics for use of the public. This data set contains 130k wine reviews published to WineEnthusiast.<img src="https://i.ibb.co/4J194vR/image-2022-01-29-193933.png" alt="enter image description here"></p>
</blockquote>
<h2 id="how-does-wine-scoring-work"><span class="prefix"></span><span class="content">How does wine scoring work?</span><span class="suffix"></span></h2>
<p>According to <em>Wine Enthusiast Magazine</em> (the source of this data), judges assign ratings based on the following scoring system:</p>

<table>
<thead>
<tr>
<th>Description</th>
<th>Rating</th>
<th>Details of the Wine Rating</th>
</tr>
</thead>
<tbody>
<tr>
<td>Classic</td>
<td><code>98-100</code></td>
<td>The pinnacle of quality</td>
</tr>
<tr>
<td>Superb</td>
<td><code>94-97</code></td>
<td>A great achievement</td>
</tr>
<tr>
<td>Excellent</td>
<td><code>90-93</code></td>
<td>Highly recommended</td>
</tr>
<tr>
<td>Very Good</td>
<td><code>87-89</code></td>
<td>Often good value; well recommended</td>
</tr>
<tr>
<td>Good</td>
<td><code>83-86</code></td>
<td>Suitable for everyday consumption</td>
</tr>
<tr>
<td>Acceptable</td>
<td><code>80-82</code></td>
<td>Can be employed in casual circumstances</td>
</tr>
</tbody>
</table><h4 id="recommended-analysis"><span class="prefix"></span><span class="content">Recommended Analysis</span><span class="suffix"></span></h4>
<ol>
<li>Which province has the highest average price?</li>
</ol>
<pre class=" language-sql"><code class="prism  language-sql"><span class="token keyword">SELECT</span> province<span class="token punctuation">,</span>
	<span class="token function">round</span><span class="token punctuation">(</span><span class="token keyword">coalesce</span><span class="token punctuation">(</span><span class="token function">avg</span><span class="token punctuation">(</span>price<span class="token punctuation">)</span><span class="token punctuation">,</span>

								<span class="token number">0</span><span class="token punctuation">)</span><span class="token punctuation">,</span>
		<span class="token number">2</span><span class="token punctuation">)</span> <span class="token keyword">AS</span> avg_price
<span class="token keyword">FROM</span> wine<span class="token punctuation">.</span>wt
<span class="token keyword">GROUP</span> <span class="token keyword">BY</span> province
<span class="token keyword">ORDER</span> <span class="token keyword">BY</span> avg_price <span class="token keyword">DESC</span>
<span class="token keyword">LIMIT</span> <span class="token number">5</span><span class="token punctuation">;</span>
</code></pre>
<p><img src="https://i.ibb.co/kKvRYmp/image-2022-01-31-151029.png" alt="enter image description here"><br>
How many reviews do Colares have?</p>
<pre class=" language-sql"><code class="prism  language-sql"><span class="token keyword">SELECT</span> description<span class="token punctuation">,</span>
	province<span class="token punctuation">,</span>
	price<span class="token punctuation">,</span>
	taster_name<span class="token punctuation">,</span>
	variety
<span class="token keyword">FROM</span> wine<span class="token punctuation">.</span>wt
<span class="token keyword">WHERE</span> province <span class="token operator">like</span> <span class="token string">'%Colares'</span><span class="token punctuation">;</span>
</code></pre>
<p>It appears that Colares only has 2 wine reviews both by the reviewer named Roger Voss. He reviewed the Malvasia which is $30 and the Ramisco which is $495.00. Colares is in Portugal as seen in the map snippet below.</p>
<p><img src="https://i.ibb.co/kq1fPLf/image-2022-01-31-151159.png" alt="Colares is in Portugal"></p>
<ol start="2">
<li>Which province has the highest average price <em>and</em> at least 10 wines?</li>
</ol>
<pre class=" language-sql"><code class="prism  language-sql"><span class="token keyword">SELECT</span> province<span class="token punctuation">,</span>
   <span class="token function">round</span><span class="token punctuation">(</span><span class="token function">avg</span><span class="token punctuation">(</span>price<span class="token punctuation">)</span><span class="token punctuation">,</span>
   	<span class="token number">2</span><span class="token punctuation">)</span> <span class="token keyword">AS</span> highest_avg_price<span class="token punctuation">,</span>
   <span class="token function">count</span><span class="token punctuation">(</span>title<span class="token punctuation">)</span> <span class="token keyword">AS</span> more_than_10_wines
<span class="token keyword">FROM</span> wine<span class="token punctuation">.</span>wt
<span class="token keyword">GROUP</span> <span class="token keyword">BY</span> province
<span class="token keyword">HAVING</span> <span class="token function">count</span><span class="token punctuation">(</span>title<span class="token punctuation">)</span> <span class="token operator">&gt;=</span> <span class="token number">10</span>
<span class="token keyword">ORDER</span> <span class="token keyword">BY</span> highest_avg_price <span class="token keyword">desc</span><span class="token punctuation">;</span>
</code></pre>
<p><img src="https://i.ibb.co/9rs31n7/image-2022-02-05-182211.png" alt="enter image description here"></p>
<p>Here are the other top Province areas with the highest average price of wine that also have 10 or more different kinds of wines.<br>
<img src="https://i.ibb.co/ftTTTJz/image-2022-02-05-182344.png" alt="enter image description here"></p>
<ol start="3">
<li>Does the number of points predict the price of the wine? If so, how strong is the correlation?</li>
</ol>
<pre class=" language-sql"><code class="prism  language-sql"><span class="token keyword">select</span> 
   corr<span class="token punctuation">(</span>points<span class="token punctuation">,</span> price<span class="token punctuation">)</span>
   	correlation_coefficient
<span class="token keyword">from</span> wine<span class="token punctuation">.</span>wt
</code></pre>
<p><img src="https://i.ibb.co/Lztqz6M/image-2022-02-09-201747.png" alt="enter image description here"></p>
<p>This correlation would be considered quite moderate with no appreciable linear correlation. So I would say no, the points do not predict the price of the wine in most cases.</p>
<ol start="4">
<li>Dig into reviewer-level trends. Visualize the distribution of reviewers by Province and by the different varieties of wine.<br>
This will be visualized as a bubble diagram.</li>
</ol>
<pre class=" language-sql"><code class="prism  language-sql"><span class="token keyword">SELECT</span> 
	taster_name<span class="token punctuation">,</span>
	<span class="token function">count</span><span class="token punctuation">(</span><span class="token keyword">distinct</span> province<span class="token punctuation">)</span> <span class="token keyword">AS</span> number_of_provinces<span class="token punctuation">,</span>
	<span class="token function">count</span><span class="token punctuation">(</span><span class="token keyword">distinct</span> variety<span class="token punctuation">)</span> <span class="token keyword">AS</span> number_of_varieties
<span class="token keyword">FROM</span> wine<span class="token punctuation">.</span>wt
<span class="token keyword">GROUP</span> <span class="token keyword">BY</span> taster_name
<span class="token keyword">ORDER</span> <span class="token keyword">BY</span>
	<span class="token function">count</span><span class="token punctuation">(</span><span class="token keyword">distinct</span> province<span class="token punctuation">)</span> <span class="token keyword">desc</span><span class="token punctuation">,</span>
	<span class="token function">count</span><span class="token punctuation">(</span><span class="token keyword">distinct</span> variety<span class="token punctuation">)</span>    
</code></pre>
<ol start="5">
<li>What was the highest rating of wine based on the wine scoring system?</li>
</ol>
<pre class=" language-sql"><code class="prism  language-sql"><span class="token comment">--Creating a table where all the points are grouped.</span>
<span class="token keyword">drop</span> <span class="token keyword">table</span> <span class="token keyword">if</span> <span class="token keyword">exists</span> point_calc<span class="token punctuation">;</span>
<span class="token keyword">create</span> <span class="token keyword">temp</span> <span class="token keyword">table</span> point_calc <span class="token keyword">as</span>
<span class="token keyword">select</span>
	points<span class="token punctuation">,</span>
	<span class="token keyword">case</span> 
		<span class="token keyword">when</span> points <span class="token operator">between</span> <span class="token number">80</span> <span class="token operator">and</span> <span class="token number">82</span> <span class="token keyword">then</span> <span class="token string">'80-82'</span>
		<span class="token keyword">when</span> points <span class="token operator">between</span> <span class="token number">83</span> <span class="token operator">and</span> <span class="token number">86</span> <span class="token keyword">then</span> <span class="token string">'83-86'</span>
		<span class="token keyword">when</span> points <span class="token operator">between</span> <span class="token number">87</span> <span class="token operator">and</span> <span class="token number">89</span> <span class="token keyword">then</span> <span class="token string">'87-89'</span>
		<span class="token keyword">when</span> points <span class="token operator">between</span> <span class="token number">90</span> <span class="token operator">and</span> <span class="token number">93</span> <span class="token keyword">then</span> <span class="token string">'90-93'</span>
		<span class="token keyword">when</span> points <span class="token operator">between</span> <span class="token number">94</span> <span class="token operator">and</span> <span class="token number">97</span> <span class="token keyword">then</span> <span class="token string">'94-97'</span>
		<span class="token keyword">when</span> points <span class="token operator">between</span> <span class="token number">98</span> <span class="token operator">and</span> <span class="token number">100</span> <span class="token keyword">then</span> <span class="token string">'98-100'</span>
		<span class="token keyword">else</span> <span class="token string">'n/a'</span> <span class="token keyword">end</span> <span class="token keyword">as</span> scoring_system
<span class="token keyword">from</span> wine<span class="token punctuation">.</span>wt<span class="token punctuation">;</span>


<span class="token comment">--Viewing the table we created with the new column conditions.</span>
<span class="token keyword">select</span> <span class="token operator">*</span> <span class="token keyword">from</span> point_calc<span class="token punctuation">;</span>


<span class="token comment">--Creating a table where we group by each grouped points category (scoring_system).</span>
<span class="token keyword">drop</span> <span class="token keyword">table</span> <span class="token keyword">if</span> <span class="token keyword">exists</span> points_calc<span class="token punctuation">;</span>
<span class="token keyword">create</span> <span class="token keyword">temp</span> <span class="token keyword">table</span> points_calc <span class="token keyword">as</span>
<span class="token keyword">select</span>
	scoring_system<span class="token punctuation">,</span>
	<span class="token function">count</span><span class="token punctuation">(</span>scoring_system<span class="token punctuation">)</span> <span class="token keyword">as</span> total
<span class="token keyword">from</span> point_calc
<span class="token keyword">group</span> <span class="token keyword">by</span> scoring_system<span class="token punctuation">;</span>


<span class="token comment">-- Viewing the final table.</span>
<span class="token keyword">select</span> <span class="token operator">*</span> <span class="token keyword">from</span> points_calc
<span class="token keyword">order</span> <span class="token keyword">by</span> total <span class="token keyword">desc</span><span class="token punctuation">;</span>
</code></pre>
<p><img src="https://i.ibb.co/VvFrj3z/image-2022-02-18-204852.png" alt="enter image description here"></p>
<p>We have a total of 129,971 rows in the raw data set and we see all the counts add up to 129,971 so no missing values of points.</p>
<p><img src="https://i.ibb.co/dMrjx7g/image-2022-02-18-204719.png" alt="enter image description here"></p>
<h2 id="code-explanation"><span class="prefix"></span><span class="content">CODE EXPLANATION</span><span class="suffix"></span></h2>
<h3 id="question-1"><span class="prefix"></span><span class="content">Question 1</span><span class="suffix"></span></h3>
<p>I use the select statement to first retrieve the province then the next column is getting the average price for each province, rounded to the 2nd decimal. I included the COALESCE () function because some of the price columns were null. I grouped by Province because it is not in an aggregate function in the select statement and I ordered by Province as well because I want to see the top 5 highest average prices per Province.</p>
<pre class=" language-sql"><code class="prism  language-sql"><span class="token keyword">SELECT</span> province<span class="token punctuation">,</span>
	<span class="token function">round</span><span class="token punctuation">(</span><span class="token keyword">coalesce</span><span class="token punctuation">(</span><span class="token function">avg</span><span class="token punctuation">(</span>price<span class="token punctuation">)</span><span class="token punctuation">,</span>

								<span class="token number">0</span><span class="token punctuation">)</span><span class="token punctuation">,</span>
		<span class="token number">2</span><span class="token punctuation">)</span> <span class="token keyword">AS</span> avg_price
<span class="token keyword">FROM</span> wine<span class="token punctuation">.</span>wt
<span class="token keyword">GROUP</span> <span class="token keyword">BY</span> province
<span class="token keyword">ORDER</span> <span class="token keyword">BY</span> avg_price <span class="token keyword">DESC</span>
<span class="token keyword">LIMIT</span> <span class="token number">5</span><span class="token punctuation">;</span>
</code></pre>
<h3 id="question-2"><span class="prefix"></span><span class="content">Question 2</span><span class="suffix"></span></h3>
<p>I used…</p>
<pre class=" language-sql"><code class="prism  language-sql"><span class="token keyword">SELECT</span> province<span class="token punctuation">,</span>
	<span class="token function">round</span><span class="token punctuation">(</span><span class="token function">avg</span><span class="token punctuation">(</span>price<span class="token punctuation">)</span><span class="token punctuation">,</span>
		<span class="token number">2</span><span class="token punctuation">)</span> <span class="token keyword">AS</span> highest_avg_price<span class="token punctuation">,</span>
	<span class="token function">count</span><span class="token punctuation">(</span>title<span class="token punctuation">)</span> <span class="token keyword">AS</span> more_than_10_wines
<span class="token keyword">FROM</span> wine<span class="token punctuation">.</span>wt
<span class="token keyword">GROUP</span> <span class="token keyword">BY</span> province
<span class="token keyword">HAVING</span> <span class="token function">count</span><span class="token punctuation">(</span>title<span class="token punctuation">)</span> <span class="token operator">&gt;=</span> <span class="token number">10</span>
<span class="token keyword">ORDER</span> <span class="token keyword">BY</span> highest_avg_price <span class="token keyword">desc</span><span class="token punctuation">;</span>
</code></pre>
<h3 id="question-3"><span class="prefix"></span><span class="content">Question 3</span><span class="suffix"></span></h3>
<p>I used …</p>
<pre class=" language-sql"><code class="prism  language-sql">query
</code></pre>
<h3 id="question-4"><span class="prefix"></span><span class="content">Question 4</span><span class="suffix"></span></h3>
<p>i Used…</p>
<pre class=" language-sql"><code class="prism  language-sql">query
</code></pre>
<h3 id="question-5"><span class="prefix"></span><span class="content">Question 5</span><span class="suffix"></span></h3>
<p>I used…</p>
<pre class=" language-sql"><code class="prism  language-sql">query
</code></pre>


---


---

<h1 id="proc-sql"><span class="prefix"></span><span class="content">PROC SQL</span><span class="suffix"></span></h1>
<h2 id="column-aliases"><span class="prefix"></span><span class="content">Column Aliases</span><span class="suffix"></span></h2>
<ul>
<li>Aliases are really important when joining tables and performing other operations with CTEs as they vastly improve SQL code readability. This also reduces the time it takes to debug and quickly scan and understand code.</li>
</ul>
<h2 id="group-by-counts"><span class="prefix"></span><span class="content">Group by Counts</span><span class="suffix"></span></h2>
<ul>
<li>Use the GROUP BY clause with a COUNT aggregate function to generate a basic frequency value counts output.<br>
|ID  |title  |category  |rating  |price</li>
</ul>

<table>
<thead>
<tr>
<th align="left">ID</th>
<th align="left">title</th>
<th align="left">category</th>
<th align="left">rating</th>
<th align="left">price</th>
</tr>
</thead>
<tbody>
<tr>
<td align="left">730</td>
<td align="left">Ridgemont</td>
<td align="left">New</td>
<td align="left">PG-13</td>
<td align="left">99</td>
</tr>
<tr>
<td align="left">892</td>
<td align="left">Titanic</td>
<td align="left">Anime</td>
<td align="left">R</td>
<td align="left">4</td>
</tr>
<tr>
<td align="left">286</td>
<td align="left">Racing</td>
<td align="left">Travel</td>
<td align="left">NC-17</td>
<td align="left">3</td>
</tr>
<tr>
<td align="left">857</td>
<td align="left">Scarface</td>
<td align="left">Comedy</td>
<td align="left">PG-13</td>
<td align="left">5</td>
</tr>
<tr>
<td align="left">593</td>
<td align="left">Labyrinth</td>
<td align="left">Horror</td>
<td align="left">G</td>
<td align="left">1</td>
</tr>
<tr>
<td align="left">664</td>
<td align="left">Roman</td>
<td align="left">Action</td>
<td align="left">PG</td>
<td align="left">2</td>
</tr>
<tr>
<td align="left">211</td>
<td align="left">Breaking</td>
<td align="left">Games</td>
<td align="left">PG-13</td>
<td align="left">5</td>
</tr>
<tr>
<td align="left">932</td>
<td align="left">Packer</td>
<td align="left">Comedy</td>
<td align="left">G</td>
<td align="left">1</td>
</tr>
<tr>
<td align="left">550</td>
<td align="left">Apache</td>
<td align="left">Family</td>
<td align="left">NC-17</td>
<td align="left">3</td>
</tr>
<tr>
<td align="left">504</td>
<td align="left">Homeward</td>
<td align="left">Drama</td>
<td align="left">PG-13</td>
<td align="left">1</td>
</tr>
</tbody>
</table><pre class=" language-sql"><code class="prism  language-sql"><span class="token comment">-- Group by query using CTE</span>
<span class="token keyword">WITH</span> table1 <span class="token keyword">as</span> <span class="token punctuation">(</span>
	<span class="token keyword">SELECT</span> 
	ID<span class="token punctuation">,</span>
	title<span class="token punctuation">,</span>
	category<span class="token punctuation">,</span>
	rating<span class="token punctuation">,</span>
	price
	<span class="token keyword">FROM</span> dvd_rentals<span class="token punctuation">.</span>films
	<span class="token keyword">LIMIT</span> <span class="token number">10</span>
<span class="token punctuation">)</span>
<span class="token keyword">SELECT</span>
	rating <span class="token punctuation">,</span>
	<span class="token function">COUNT</span><span class="token punctuation">(</span><span class="token operator">*</span><span class="token punctuation">)</span> <span class="token keyword">as</span> record_count
<span class="token keyword">FROM</span> table1
<span class="token keyword">GROUP</span> <span class="token keyword">BY</span> rating
<span class="token keyword">ORDER</span> <span class="token keyword">BY</span> record_count <span class="token keyword">DESC</span><span class="token punctuation">;</span>
</code></pre>
<h2 id="modified-window-function"><span class="prefix"></span><span class="content">Modified Window Function</span><span class="suffix"></span></h2>
<ul>
<li><code>::Numeric</code> right after the <code>COUNT(*)</code> is to avoid the integer floor division.</li>
</ul>
<pre class=" language-sql"><code class="prism  language-sql"><span class="token keyword">SELECT</span>
	rating<span class="token punctuation">,</span>
	<span class="token function">COUNT</span><span class="token punctuation">(</span><span class="token operator">*</span><span class="token punctuation">)</span> <span class="token keyword">as</span> frequency<span class="token punctuation">,</span>
	<span class="token function">ROUND</span><span class="token punctuation">(</span><span class="token number">100</span> <span class="token operator">*</span> <span class="token function">COUNT</span><span class="token punctuation">(</span><span class="token operator">*</span><span class="token punctuation">)</span>::<span class="token keyword">NUMERIC</span> <span class="token operator">/</span> <span class="token function">SUM</span><span class="token punctuation">(</span><span class="token function">COUNT</span><span class="token punctuation">(</span><span class="token operator">*</span><span class="token punctuation">)</span><span class="token punctuation">)</span> <span class="token keyword">OVER</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">,</span> <span class="token number">2</span><span class="token punctuation">)</span> <span class="token keyword">as</span> percentage
<span class="token keyword">FROM</span> dvd_rentals<span class="token punctuation">.</span>film_list
<span class="token keyword">GROUP</span> <span class="token keyword">BY</span> rating
<span class="token keyword">ORDER</span> <span class="token keyword">BY</span> frequency <span class="token keyword">DESC</span><span class="token punctuation">;</span>
</code></pre>
<blockquote>
<p><strong>PostgreSQL</strong> does not allow <code>COUNT(DISTINCT *)</code></p>
</blockquote>
<h2 id="common-table-expression"><span class="prefix"></span><span class="content">Common Table Expression</span><span class="suffix"></span></h2>
<ul>
<li></li>
</ul>


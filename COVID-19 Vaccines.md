---


---

<h1 id="u.s.-covid-vaccines"><span class="prefix"></span><span class="content">U.S. COVID Vaccines</span><span class="suffix"></span></h1>
<blockquote>
<p><a href="https://ourworldindata.org/us-states-vaccinations">US COVID-19 Vaccination Data</a><img src="https://i.ibb.co/Dfsy8Ts/image-2022-01-22-192312.png" alt="enter image description here"></p>
</blockquote>
<h1 id="eda"><span class="prefix"></span><span class="content">EDA</span><span class="suffix"></span></h1>
<p>How many states participated in reporting vaccination numbers? <em>Might need to remove the U.S. territories</em></p>
<pre class=" language-sql"><code class="prism  language-sql"><span class="token keyword">select</span>
<span class="token function">count</span> <span class="token punctuation">(</span><span class="token keyword">distinct</span> location<span class="token punctuation">)</span>
<span class="token keyword">from</span> vax<span class="token punctuation">.</span>covid<span class="token punctuation">;</span>
</code></pre>
<p><strong>64</strong></p>
<ol>
<li>How many COVID-19 vaccine doses have been  <strong>administered</strong>?</li>
</ol>
<pre class=" language-sql"><code class="prism  language-sql"><span class="token keyword">select</span> location<span class="token punctuation">,</span> <span class="token keyword">coalesce</span><span class="token punctuation">(</span><span class="token function">max</span><span class="token punctuation">(</span>total_vaccinations<span class="token punctuation">)</span><span class="token punctuation">,</span><span class="token number">0</span><span class="token punctuation">)</span> <span class="token keyword">as</span> doses_administered
<span class="token keyword">from</span> vax<span class="token punctuation">.</span>covid
<span class="token keyword">group</span> <span class="token keyword">by</span> location
<span class="token keyword">order</span> <span class="token keyword">by</span> location<span class="token punctuation">;</span>
</code></pre>
<p><img src="https://i.ibb.co/16Q3yGx/image-2022-01-22-171634.png" alt="enter image description here"><br>
2.  What share of the population has received  <strong>at least one dose</strong>  of the COVID-19 vaccine?</p>
<pre class=" language-sql"><code class="prism  language-sql"><span class="token keyword">select</span> location<span class="token punctuation">,</span> <span class="token keyword">coalesce</span><span class="token punctuation">(</span><span class="token function">max</span><span class="token punctuation">(</span>people_vaccinated<span class="token punctuation">)</span><span class="token punctuation">,</span><span class="token number">0</span><span class="token punctuation">)</span> <span class="token keyword">as</span> at_least_one_dose_recieved
<span class="token keyword">from</span> vax<span class="token punctuation">.</span>covid
<span class="token keyword">group</span> <span class="token keyword">by</span> location
<span class="token keyword">order</span> <span class="token keyword">by</span> location<span class="token punctuation">;</span>
</code></pre>
<p><img src="https://i.ibb.co/b5kCk9w/image-2022-01-22-172014.png" alt="enter image description here"><br>
3.  What share of the population has been  <strong>fully vaccinated</strong>  against COVID-19?</p>
<pre class=" language-sql"><code class="prism  language-sql"><span class="token keyword">select</span> location<span class="token punctuation">,</span> <span class="token function">max</span><span class="token punctuation">(</span>people_fully_vaccinated<span class="token punctuation">)</span> 
<span class="token keyword">from</span> vax<span class="token punctuation">.</span>covid
<span class="token keyword">group</span> <span class="token keyword">by</span> location
<span class="token keyword">order</span> <span class="token keyword">by</span> location<span class="token punctuation">;</span>
</code></pre>
<p><img src="https://i.ibb.co/3fpNRDh/image-2022-01-22-174804.png" alt="enter image description here"><br>
4. How many COVID-19 vaccine doses are administered  <strong>daily</strong>? <em>based on a 7-day rolling average</em> The code below does not actually work, still in the process of correcting it.</p>
<pre class=" language-sql"><code class="prism  language-sql"><span class="token keyword">select</span> location<span class="token punctuation">,</span> <span class="token keyword">date</span><span class="token punctuation">,</span> daily_vaccinations<span class="token punctuation">,</span> <span class="token function">round</span> <span class="token punctuation">(</span>
<span class="token function">avg</span><span class="token punctuation">(</span>daily_vaccinations<span class="token punctuation">)</span> <span class="token keyword">over</span><span class="token punctuation">(</span><span class="token keyword">order</span> <span class="token keyword">by</span> <span class="token keyword">date</span> range <span class="token operator">between</span> <span class="token string">'6 days'</span> <span class="token keyword">preceding</span> <span class="token operator">and</span> <span class="token keyword">current</span> <span class="token keyword">row</span><span class="token punctuation">)</span><span class="token punctuation">,</span>
	<span class="token number">2</span><span class="token punctuation">)</span>
<span class="token keyword">as</span> ma7
<span class="token keyword">from</span> vax<span class="token punctuation">.</span>covid
<span class="token keyword">order</span> <span class="token keyword">by</span> location<span class="token punctuation">,</span> <span class="token keyword">date</span> <span class="token keyword">desc</span><span class="token punctuation">;</span>
</code></pre>
<p><img src="https://i.ibb.co/fxYbCRB/image-2022-01-22-190708.png" alt="enter image description here"><br>
5. How many COVID-19 vaccine doses have been  <strong>distributed</strong>? What share of distributed vaccines has been used?</p>
<pre class=" language-sql"><code class="prism  language-sql"><span class="token keyword">select</span> <span class="token function">max</span><span class="token punctuation">(</span><span class="token keyword">date</span><span class="token punctuation">)</span><span class="token punctuation">,</span> location<span class="token punctuation">,</span> <span class="token function">max</span><span class="token punctuation">(</span>total_distributed<span class="token punctuation">)</span>
<span class="token keyword">from</span> vax<span class="token punctuation">.</span>covid
<span class="token keyword">group</span> <span class="token keyword">by</span> location
<span class="token keyword">order</span> <span class="token keyword">by</span> location<span class="token punctuation">;</span>
</code></pre>
<p><img src="https://i.ibb.co/QCN43Wh/image-2022-01-22-191714.png" alt="enter image description here"><br>
6. How many total vaccinations by state?<br>
7. Across the nation, how many vaccinations per month? Which month saw the most vaccinations?</p>
<p><strong>Percentage of people who received at least one dose of the vaccine</strong></p>
<pre class=" language-sql"><code class="prism  language-sql"><span class="token keyword">create</span> <span class="token keyword">temp</span> <span class="token keyword">table</span> total_vaxxed <span class="token keyword">AS</span>
<span class="token keyword">select</span>
	people_vaccinated_per_hundred<span class="token punctuation">,</span>
	location<span class="token punctuation">,</span>
	<span class="token keyword">coalesce</span><span class="token punctuation">(</span><span class="token function">max</span><span class="token punctuation">(</span>people_vaccinated<span class="token punctuation">)</span><span class="token punctuation">,</span><span class="token number">0</span><span class="token punctuation">)</span> <span class="token keyword">as</span> at_least_one_dose_recieved
<span class="token keyword">from</span> vax<span class="token punctuation">.</span>covid
<span class="token keyword">group</span> <span class="token keyword">by</span> location<span class="token punctuation">,</span> people_vaccinated_per_hundred
<span class="token keyword">order</span> <span class="token keyword">by</span> location<span class="token punctuation">;</span>

<span class="token keyword">select</span> location<span class="token punctuation">,</span> <span class="token function">round</span> <span class="token punctuation">(</span>
	<span class="token number">100</span> <span class="token operator">*</span> at_least_one_dose_recieved :: <span class="token keyword">numeric</span> <span class="token operator">/</span> <span class="token function">sum</span><span class="token punctuation">(</span><span class="token function">max</span><span class="token punctuation">(</span>people_vaccinated_per_hundred<span class="token punctuation">)</span><span class="token punctuation">)</span> <span class="token keyword">over</span> <span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">,</span>
	<span class="token number">2</span><span class="token punctuation">)</span>
	<span class="token keyword">as</span> percentage
<span class="token keyword">from</span> total_vaxxed
<span class="token keyword">group</span> <span class="token keyword">by</span> location<span class="token punctuation">,</span> at_least_one_dose_recieved
<span class="token keyword">order</span> <span class="token keyword">by</span> location<span class="token punctuation">;</span>
</code></pre>
<p><strong>Percent change sql code</strong></p>
<pre class=" language-sql"><code class="prism  language-sql"><span class="token keyword">with</span> cte <span class="token keyword">as</span> <span class="token punctuation">(</span>
<span class="token keyword">select</span> <span class="token keyword">date</span><span class="token punctuation">,</span> people_fully_vaccinated<span class="token punctuation">,</span>
row_number<span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token keyword">over</span><span class="token punctuation">(</span><span class="token keyword">order</span> <span class="token keyword">by</span> <span class="token keyword">date</span><span class="token punctuation">)</span> <span class="token keyword">as</span> rn1
<span class="token keyword">from</span> vax<span class="token punctuation">.</span>covid<span class="token punctuation">)</span>

<span class="token keyword">select</span> t1<span class="token punctuation">.</span><span class="token keyword">date</span><span class="token punctuation">,</span> t1<span class="token punctuation">.</span>people_fully_vaccinated<span class="token punctuation">,</span>
<span class="token function">round</span><span class="token punctuation">(</span><span class="token keyword">coalesce</span><span class="token punctuation">(</span><span class="token punctuation">(</span>t1<span class="token punctuation">.</span>people_fully_vaccinated<span class="token operator">-</span>t2<span class="token punctuation">.</span>people_fully_vaccinated<span class="token punctuation">)</span><span class="token operator">*</span><span class="token number">1.0</span><span class="token operator">/</span><span class="token keyword">nullif</span><span class="token punctuation">(</span>t2<span class="token punctuation">.</span>people_fully_vaccinated<span class="token punctuation">,</span><span class="token number">0</span><span class="token punctuation">)</span><span class="token punctuation">,</span><span class="token number">0</span><span class="token punctuation">)</span><span class="token operator">*</span><span class="token number">100</span><span class="token punctuation">,</span><span class="token number">2</span><span class="token punctuation">)</span>
<span class="token keyword">from</span> cte t1
<span class="token keyword">left</span> <span class="token keyword">join</span> cte t2 <span class="token keyword">on</span> t1<span class="token punctuation">.</span>rn1 <span class="token operator">=</span> t2<span class="token punctuation">.</span>rn1<span class="token operator">+</span><span class="token number">1</span>
<span class="token punctuation">`</span><span class="token punctuation">`</span><span class="token punctuation">`</span>ith <span class="token punctuation">[</span>StackEdit<span class="token operator">+</span><span class="token punctuation">]</span><span class="token punctuation">(</span>https:<span class="token comment">//stackedit.net/).</span>
</code></pre>


---


---

<h1 id="new-years-eve-resolutions"><span class="prefix"></span><span class="content">New Year’s Eve Resolutions</span><span class="suffix"></span></h1>
<blockquote>
<p><strong>Note:</strong> <em>This dataset was downloaded from Maven Analytics website using the New Years Eve Resolution data.</em><br>
<img src="https://i.ibb.co/w0vPbPJ/image-2021-12-12-224902.png" alt="enter image description here"></p>
</blockquote>
<p><a href="https://www.mavenanalytics.io/data-playground?search=new%20year">https://www.mavenanalytics.io/data-playground?search=new year</a></p>
<ul>
<li>
<p>We’re looking at tweets from 2015 containing New Year’s resolutions</p>
</li>
<li>
<p>Each record represents one tweet</p>
</li>
<li>
<p>Records contain the date &amp; time of the tweet, geographic location, full text, and category</p>
</li>
<li>
<p>The sources of this data is data.world</p>
</li>
</ul>
<h2 id="recommended-analysis-from-maven-analytics"><span class="prefix"></span><span class="content">Recommended Analysis from Maven Analytics</span><span class="suffix"></span></h2>
<blockquote>
<p><em>Below I answer each question followed with the SQL query to answer the question and its corresponding output from PostgreSQL.</em></p>
</blockquote>
<ol>
<li>What is the most popular resolution category? Least popular?  <strong>The most popular resolution category is Personal Growth and the least popular is Philanthropic.</strong></li>
</ol>
<pre class=" language-sql"><code class="prism  language-sql"><span class="token keyword">SELECT</span> category<span class="token punctuation">,</span>
	<span class="token function">COUNT</span> <span class="token punctuation">(</span><span class="token operator">*</span><span class="token punctuation">)</span> <span class="token keyword">AS</span> category_frequency
<span class="token keyword">FROM</span> resolutions
<span class="token keyword">GROUP</span> <span class="token keyword">BY</span> category
<span class="token keyword">ORDER</span> <span class="token keyword">BY</span> category_frequency <span class="token keyword">DESC</span><span class="token punctuation">;</span>
</code></pre>
<p><img src="https://i.ibb.co/NtdQ9MP/image-2021-12-12-195611.png" alt="enter image description here"></p>
<ol start="2">
<li>Which resolution category was retweeted the most? Least? <strong>The category that was retweeted the most was Finance and the least was Career.</strong></li>
</ol>
<pre class=" language-sql"><code class="prism  language-sql"><span class="token keyword">SELECT</span> category<span class="token punctuation">,</span> <span class="token function">max</span><span class="token punctuation">(</span>retweets<span class="token punctuation">)</span> <span class="token keyword">as</span> highest_retweets
<span class="token keyword">FROM</span> resolutions
<span class="token keyword">GROUP</span> <span class="token keyword">BY</span> retweets<span class="token punctuation">,</span> category
<span class="token keyword">ORDER</span> <span class="token keyword">BY</span> highest_retweets <span class="token keyword">DESC</span><span class="token punctuation">;</span>
</code></pre>
<p><img src="https://i.ibb.co/2MvfH31/image-2021-12-12-202144.png" alt="enter image description here"></p>
<ol start="3">
<li>Using the tweet_created field, and rounding to the nearest hour, what was the most popular hour of the day to tweet? <strong>9:00 pm was the most popular hour to tweet.</strong> How many resolutions were tweeted? <strong>505 resolutions were tweeted on or around 9:00 pm.</strong></li>
</ol>
<pre class=" language-sql"><code class="prism  language-sql"><span class="token keyword">SELECT</span> date_trunc<span class="token punctuation">(</span><span class="token string">'hour'</span><span class="token punctuation">,</span> time<span class="token punctuation">)</span> <span class="token keyword">AS</span> rounded_time<span class="token punctuation">,</span>
	<span class="token function">COUNT</span><span class="token punctuation">(</span>date_trunc<span class="token punctuation">(</span><span class="token string">'hour'</span><span class="token punctuation">,</span> time<span class="token punctuation">)</span><span class="token punctuation">)</span> <span class="token keyword">AS</span> count_of_time
<span class="token keyword">FROM</span> resolutions
<span class="token keyword">GROUP</span> <span class="token keyword">BY</span> rounded_time
<span class="token keyword">ORDER</span> <span class="token keyword">BY</span> count_of_time <span class="token keyword">DESC</span><span class="token punctuation">;</span>
</code></pre>
<p><img src="https://i.ibb.co/phPKGg4/image-2021-12-12-231030.png" alt="enter image description here"><br>
4. Using a map visual, what U.S. State tweeted the highest number of NYE resolutions? <strong>California is the U.S. State that had the highest number of NYE resolutions.</strong><br>
<img src="https://i.ibb.co/vjB1X0T/image-2021-12-12-233921.png" alt="enter image description here">://stackedit.net/).</p>


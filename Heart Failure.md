---


---

<h1 id="heart-failure-dataset"><span class="prefix"></span><span class="content">Heart Failure Dataset</span><span class="suffix"></span></h1>
<blockquote>
<p><a href="https://www.kaggle.com/fedesoriano/heart-failure-prediction">https://www.kaggle.com/fedesoriano/heart-failure-prediction</a><img src="https://i.ibb.co/sgYqmf1/image-2021-12-29-155153.png" alt="enter image description here"></p>
</blockquote>
<h1 id="eda"><span class="prefix"></span><span class="content">EDA</span><span class="suffix"></span></h1>
<p>What factors likely contribute to heart diesease?</p>
<ol>
<li>what gender had higher diagnoses of heart failure? <strong>Male</strong></li>
<li>what blood pressure level do we start to see heart failure?</li>
<li>what are the cholesterol levels and heart failure cases of each age group?</li>
<li>what is the major chest pain type that leads to heart failure?</li>
</ol>
<h2 id="helpful-information"><span class="prefix"></span><span class="content">Helpful information</span><span class="suffix"></span></h2>
<p>Angina is the term for chest pain caused by poor blood flow to the heart. This is often caused by the buildup of thick plaques on the inner walls of the arteries that carry blood to the heart.</p>
<p><strong>Column information</strong> In the chestpaintypr column<br>
ChestPainType: chest pain type [TA: Typical Angina, ATA: Atypical Angina, NAP: Non-Anginal Pain, ASY: Asymptomatic]</p>
<p>fastingbs: fasting blood sugar [1: if FastingBS &gt; 120 mg/dl, 0: otherwise]</p>
<p>RestingBP: resting blood pressure [mm Hg]</p>
<p><strong>Historical information</strong> Women may have more of a subtle presentation called atypical angina.</p>
<p>credits: <a href="https://www.harringtonhospital.org/typical-and-atypical-angina-what-to-look-for/">https://www.harringtonhospital.org/typical-and-atypical-angina-what-to-look-for/</a></p>
<p>Normal: Blood pressure <strong>below 120/80 mm Hg</strong> is considered to be normal. Elevated: When blood pressure readings consistently range from 120 to 129 systolic and less than 80 mm Hg diastolic, it is known as elevated blood pressure</p>
<p>An ideal total cholesterol level is <strong>lower than 200 mg/dL</strong>. Anything between 200 and 239 mg/dL is borderline, and anything above 240 mg/dL is high.</p>
<p><img src="https://i.ibb.co/7KKHtZy/image-2021-12-29-174243.png" alt="enter image description here"></p>
<p><img src="https://i.ibb.co/bbtCLPc/image-2021-12-29-174936.png" alt="enter image description here"></p>
<p>1= yes<br>
0= no</p>
<h2 id="analysis"><span class="prefix"></span><span class="content">Analysis</span><span class="suffix"></span></h2>
<ul>
<li>Total number of people who have heart failure.</li>
</ul>
<pre class=" language-sql"><code class="prism  language-sql"><span class="token keyword">SELECT</span> HEARTDISEASE<span class="token punctuation">,</span>
	<span class="token function">COUNT</span><span class="token punctuation">(</span><span class="token operator">*</span><span class="token punctuation">)</span> <span class="token keyword">AS</span> COUNTS
<span class="token keyword">FROM</span> HEART<span class="token punctuation">.</span>FAILURE
<span class="token keyword">GROUP</span> <span class="token keyword">BY</span> HEARTDISEASE
<span class="token keyword">ORDER</span> <span class="token keyword">BY</span> COUNTS <span class="token keyword">DESC</span><span class="token punctuation">;</span>
</code></pre>
<p><img src="https://i.ibb.co/Ph7RrVc/image-2021-12-29-180302.png" alt="enter image description here"><br>
<strong>508</strong></p>
<hr>
<ul>
<li>Find the percentage of heart failure cases by chest pain type. Shown as a pie chart.</li>
</ul>
<pre class=" language-sql"><code class="prism  language-sql"><span class="token keyword">SELECT</span> CHESTPAINTYPE<span class="token punctuation">,</span>
	HEARTDISEASE<span class="token punctuation">,</span>
	<span class="token function">COUNT</span><span class="token punctuation">(</span><span class="token operator">*</span><span class="token punctuation">)</span> <span class="token keyword">AS</span> COUNTS<span class="token punctuation">,</span>
	<span class="token function">ROUND</span><span class="token punctuation">(</span><span class="token number">100</span> <span class="token operator">*</span> <span class="token function">COUNT</span><span class="token punctuation">(</span><span class="token operator">*</span><span class="token punctuation">)</span> <span class="token operator">/</span> <span class="token function">SUM</span><span class="token punctuation">(</span><span class="token function">COUNT</span><span class="token punctuation">(</span><span class="token operator">*</span><span class="token punctuation">)</span><span class="token punctuation">)</span> <span class="token keyword">OVER</span> <span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">,</span> <span class="token number">2</span><span class="token punctuation">)</span> <span class="token keyword">AS</span> PERCENTAGE
<span class="token keyword">FROM</span> HEART<span class="token punctuation">.</span>FAILURE
<span class="token keyword">GROUP</span> <span class="token keyword">BY</span> CHESTPAINTYPE<span class="token punctuation">,</span>
	HEARTDISEASE
<span class="token keyword">ORDER</span> <span class="token keyword">BY</span> COUNTS <span class="token keyword">DESC</span><span class="token punctuation">;</span>
</code></pre>
<p>ASY  <strong>Asymptomatic</strong><br>
TA <strong>Typical Angina</strong><br>
ATA <strong>Atypical Angina</strong><br>
NAP <strong>Non-Anginal Pain</strong></p>
<p><img src="https://i.ibb.co/QrbtkxP/piechart.png" alt="enter image description here"></p>
<hr>
<ul>
<li>who has good cholesterol and bad cholesterol in comparison to who has heart failure?<br>
An ideal total cholesterol level is <strong>lower than 200 mg/dL</strong>. Anything between 200 and 239 mg/dL is borderline, and anything above 240 mg/dL is high.</li>
</ul>
<pre class=" language-sql"><code class="prism  language-sql"><span class="token keyword">SELECT</span> SEX<span class="token punctuation">,</span>
	HEARTDISEASE<span class="token punctuation">,</span>
	CHOLESTEROL
<span class="token keyword">FROM</span> HEART<span class="token punctuation">.</span>FAILURE
<span class="token keyword">ORDER</span> <span class="token keyword">BY</span> CHOLESTEROL <span class="token keyword">DESC</span><span class="token punctuation">,</span>
	HEARTDISEASE <span class="token keyword">DESC</span><span class="token punctuation">,</span>
	SEX<span class="token punctuation">;</span>
</code></pre>
<p><img src="https://i.ibb.co/YLXWx8G/image-2021-12-31-124142.png" alt="enter image description here"></p>
<ul>
<li>what gender has more heart failure</li>
</ul>
<pre class=" language-sql"><code class="prism  language-sql"><span class="token keyword">SELECT</span> SEX<span class="token punctuation">,</span>
	<span class="token function">COUNT</span><span class="token punctuation">(</span>SEX<span class="token punctuation">)</span><span class="token punctuation">,</span>
	HEARTDISEASE
<span class="token keyword">FROM</span> HEART<span class="token punctuation">.</span>FAILURE
<span class="token keyword">GROUP</span> <span class="token keyword">BY</span> SEX<span class="token punctuation">,</span>
	HEARTDISEASE<span class="token punctuation">;</span>
</code></pre>
<p><img src="https://i.ibb.co/WKkw2p4/image-2021-12-31-124430.png" alt="enter image description here"></p>
<ul>
<li>What blood pressure do we start to see heart failure?</li>
</ul>
<pre class=" language-sql"><code class="prism  language-sql"><span class="token keyword">SELECT</span> HEARTDISEASE<span class="token punctuation">,</span>
	RESTINGBP
<span class="token keyword">FROM</span> HEART<span class="token punctuation">.</span>FAILURE
<span class="token keyword">ORDER</span> <span class="token keyword">BY</span> HEARTDISEASE<span class="token punctuation">,</span>
	RESTINGBP <span class="token keyword">DESC</span><span class="token punctuation">;</span>
</code></pre>
<p><img src="https://i.ibb.co/mGpjnLn/image-2021-12-31-130340.png" alt="enter image description here"></p>
<ul>
<li>what are the cholesterol levels and heart failure cases of each age group?</li>
</ul>
<pre class=" language-sql"><code class="prism  language-sql"><span class="token keyword">SELECT</span> AGE<span class="token punctuation">,</span>
	CHOLESTEROL<span class="token punctuation">,</span>
	HEARTDISEASE
<span class="token keyword">FROM</span> HEART<span class="token punctuation">.</span>FAILURE
<span class="token keyword">GROUP</span> <span class="token keyword">BY</span> AGE<span class="token punctuation">,</span>
	HEARTDISEASE<span class="token punctuation">,</span>
	CHOLESTEROL
<span class="token keyword">ORDER</span> <span class="token keyword">BY</span> AGE <span class="token keyword">DESC</span><span class="token punctuation">,</span>
	HEARTDISEASE <span class="token keyword">DESC</span><span class="token punctuation">;</span>
</code></pre>
<p><img src="https://i.ibb.co/VC0fyvN/image-2021-12-31-131621.png" alt="enter image description here"></p>
<p>The Data Analyst is responsible for providing accurate, reliable and accessible business metrics as well as contextualizing them in operational performance with research and advanced analysis. The Data Analyst will work closely with Analytics Business Partners and Research Scientists to improve and scale our People Data work. The role will also liaise with various cross-functional partners across People Data teams and Data Engineer stakeholders to effectively organize and distill data, ask appropriate questions to understand hypotheses and requests, and visualize and present complex data in a compelling manner to an Executive audience. We are looking for Data Analysts who share our passion for building new functionality and improving existing systems. The Candidate will be curious and inquisitive, have strong critical thinking and analytical skills, will thrive managing concurrent projects while working with a global team to drive outcomes and will enjoy informing and influencing stakeholders with data and insights. The candidate will also have the ability to build strong partnerships to drive impact across the organization.</p>
<p>People Data Analyst Responsibilities</p>
<ul>
<li>
<p>Build new analytics and reporting capabilities to support program evaluation and operations.</p>
</li>
<li>
<p>Handle ad hoc reporting and analytics requests, while addressing Stakeholder long-term needs and driving insights for HR Partners and Executives using existing dashboard, HRIS data, and leveraging our people data ecosystem.</p>
</li>
<li>
<p>Intake on business needs and apply contextual knowledge in order to scope data requests, synthesize insights, and recommend solutions to key business partners in collaboration with others.</p>
</li>
<li>
<p>Effectively communicate with cross-functional partners and internal teams to deliver solutions in a timely fashion.</p>
</li>
<li>
<p>Proactively manage stakeholder expectations, manage escalations and resolve issues in a timely manner.</p>
</li>
</ul>
<p>Minimum Qualifications</p>
<ul>
<li>
<p>3+ years of experience working with SQL or relational database</p>
</li>
<li>
<p>2+ years of experience with data visualization tools</p>
</li>
<li>
<p>Experience manipulating large data sets through object oriented programming (ex: Python), statistical software ® or other methods</p>
</li>
<li>
<p>Experience initiating and driving projects to completion with minimal guidance</p>
</li>
<li>
<p>Experience having effective conversations with clients about their support needs and requirements, managing the intake process, and asking the right questions to scope and solve the requests</p>
</li>
<li>
<p>Demonstrate judgment and discretion when dealing with highly sensitive people data and business issues</p>
</li>
</ul>
<p>Preferred Qualifications</p>
<ul>
<li>
<p>Comfortable working in a fast-paced and demanding environment</p>
</li>
<li>
<p>Experience with R or Python</p>
</li>
<li>
<p>Experience working with HR/organizational people data preferred (e.g., headcount, turnover, recruiting metrics, and other people analytics)</p>
</li>
<li>
<p>Experience with data management, specifically with building data systems and validating reports from disparate <a href="http://systemstackedit.net/">systemstackedit.net/</a>).</p>
</li>
</ul>


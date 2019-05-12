---


---

<h1 id="ocaml">OCaml</h1>
<h2 id="records">Records</h2>
<pre class=" language-ocaml"><code class="prism  language-ocaml">	<span class="token keyword">type</span> date <span class="token operator">=</span> <span class="token punctuation">{</span> month<span class="token punctuation">:</span> string<span class="token punctuation">,</span> day<span class="token punctuation">:</span> int<span class="token punctuation">,</span> year<span class="token punctuation">:</span> int <span class="token punctuation">}</span><span class="token punctuation">;</span><span class="token punctuation">;</span> 		<span class="token comment">(* fields are accessible by name *)</span>
	<span class="token keyword">let</span> today <span class="token operator">=</span> <span class="token punctuation">{</span> day <span class="token operator">=</span> <span class="token number">16</span><span class="token punctuation">;</span> year <span class="token operator">=</span> <span class="token number">2017</span><span class="token punctuation">;</span> month <span class="token operator">=</span> <span class="token string">"f"</span><span class="token operator">^</span><span class="token string">"eb"</span> <span class="token punctuation">}</span><span class="token punctuation">;</span><span class="token punctuation">;</span>
	today<span class="token punctuation">.</span>day<span class="token punctuation">;</span><span class="token punctuation">;</span>													<span class="token comment">(* fields accessible by dot notation *)</span>
	<span class="token keyword">let</span> <span class="token punctuation">{</span> day <span class="token operator">=</span> d <span class="token punctuation">}</span> <span class="token operator">=</span> today<span class="token punctuation">;</span><span class="token punctuation">;</span>									<span class="token comment">(* fields accessible by pattern matching (d has the value 16) *)</span>
</code></pre>


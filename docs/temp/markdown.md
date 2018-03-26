# markdown 에서의 수식 사용
mathjax를 이용하자. 아래 script 이용.

```
<script type="text/javascript"src="http://cdn.mathjax.org/mathjax/latest/MathJax.js">
MathJax.Hub.Config({extensions: ["tex2jax.js","TeX/AMSmath.js","TeX/AMSsymbols.js"],
jax: ["input/TeX", "output/HTML-CSS"],
tex2jax: {
inlineMath: [ ['$','$'], ["\\(","\\)"] ],
displayMath: [ ['$$','$$'], ["\\[","\\]"] ],
processEscapes: true
},
"HTML-CSS": { availableFonts: ["TeX"] }
});
</script>
```

참고로, $100 와 같이 일반적인 용도의 dollor을 사용하기 위해서는 `processEscapes: true` 로 세팅해주어야 한다. (`\$` 로 표현)


<img src="http://upload-images.jianshu.io/upload_images/1504317-805392e6d4ad6f43.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240" data-original-src="http://upload-images.jianshu.io/upload_images/1504317-805392e6d4ad6f43.jpg?imageMogr2/auto-orient/strip%7CimageView2/2" style="cursor: zoom-in;"><br><div class="image-caption"></div>
</div><p><br>ECMAScript 6（以下简称ES6）是JavaScript语言的下一代标准。因为当前版本的ES6是在2015年发布的，所以又称ECMAScript 2015。</p>
<p>也就是说，ES6就是ES2015。</p>
<p>虽然目前并不是所有浏览器都能兼容ES6全部特性，但越来越多的程序员在实际项目当中已经开始使用ES6了。所以就算你现在不打算使用ES6，但为了看懂别人的你也该懂点ES6的语法了...</p>
<p>在我们正式讲解ES6语法之前，我们得先了解下Babel。<br><a href="https://babeljs.io/" target="_blank">Babel</a></p>
<p>Babel是一个广泛使用的ES6转码器，可以将ES6代码转为ES5代码，从而在现有环境执行。大家可以选择自己习惯的工具来使用使用Babel，具体过程可直接在Babel官网查看：<br></p><div class="image-package">
<img src="http://upload-images.jianshu.io/upload_images/1504317-d020f21868e8e84c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240" data-original-src="http://upload-images.jianshu.io/upload_images/1504317-d020f21868e8e84c.png?imageMogr2/auto-orient/strip%7CimageView2/2" style="cursor: zoom-in;"><br><div class="image-caption">https://babeljs.io/docs/setup/</div>
</div>
<h3>最常用的ES6特性</h3>
<p><code>let, const, class, extends, super, arrow functions, template string, destructuring, default, rest arguments</code><br>这些是ES6最常用的几个语法，基本上学会它们，我们就可以走遍天下都不怕啦！我会用最通俗易懂的语言和例子来讲解它们，保证一看就懂，一学就会。</p>
<h3>let, const</h3>
<p>这两个的用途与<code>var</code>类似，都是用来声明变量的，但在实际运用中他俩都有各自的特殊用途。<br>首先来看下面这个例子：</p>
<pre class="hljs sqf"><code class="sqf">var <span class="hljs-built_in">name</span> = <span class="hljs-string">'zach'</span>

<span class="hljs-keyword">while</span> (<span class="hljs-literal">true</span>) {
    var <span class="hljs-built_in">name</span> = <span class="hljs-string">'obama'</span>
    console.<span class="hljs-built_in">log</span>(<span class="hljs-built_in">name</span>)  <span class="hljs-comment">//obama</span>
    break
}

console.<span class="hljs-built_in">log</span>(<span class="hljs-built_in">name</span>)  <span class="hljs-comment">//obama</span></code></pre>
<p>使用<code>var</code>两次输出都是obama，这是因为ES5只有全局作用域和函数作用域，没有块级作用域，这带来很多不合理的场景。第一种场景就是你现在看到的内层变量覆盖外层变量。而<code>let</code>则实际上为JavaScript新增了块级作用域。用它所声明的变量，只在<code>let</code>命令所在的代码块内有效。</p>
<pre class="hljs sqf"><code class="sqf">let <span class="hljs-built_in">name</span> = <span class="hljs-string">'zach'</span>

<span class="hljs-keyword">while</span> (<span class="hljs-literal">true</span>) {
    let <span class="hljs-built_in">name</span> = <span class="hljs-string">'obama'</span>
    console.<span class="hljs-built_in">log</span>(<span class="hljs-built_in">name</span>)  <span class="hljs-comment">//obama</span>
    break
}

console.<span class="hljs-built_in">log</span>(<span class="hljs-built_in">name</span>)  <span class="hljs-comment">//zach</span></code></pre>
<p>另外一个<code>var</code>带来的不合理场景就是用来计数的循环变量泄露为全局变量，看下面的例子：</p>
<pre class="hljs javascript"><code class="javascript"><span class="hljs-keyword">var</span> a = [];
<span class="hljs-keyword">for</span> (<span class="hljs-keyword">var</span> i = <span class="hljs-number">0</span>; i &lt; <span class="hljs-number">10</span>; i++) {
  a[i] = <span class="hljs-function"><span class="hljs-keyword">function</span> (<span class="hljs-params"></span>) </span>{
    <span class="hljs-built_in">console</span>.log(i);
  };
}
a[<span class="hljs-number">6</span>](); <span class="hljs-comment">// 10</span></code></pre>
<p>上面代码中，变量i是var声明的，在全局范围内都有效。所以每一次循环，新的i值都会覆盖旧值，导致最后输出的是最后一轮的i的值。而使用let则不会出现这个问题。</p>
<pre class="hljs javascript"><code class="javascript"><span class="hljs-keyword">var</span> a = [];
<span class="hljs-keyword">for</span> (<span class="hljs-keyword">let</span> i = <span class="hljs-number">0</span>; i &lt; <span class="hljs-number">10</span>; i++) {
  a[i] = <span class="hljs-function"><span class="hljs-keyword">function</span> (<span class="hljs-params"></span>) </span>{
    <span class="hljs-built_in">console</span>.log(i);
  };
}
a[<span class="hljs-number">6</span>](); <span class="hljs-comment">// 6</span></code></pre>
<p>再来看一个更常见的例子，了解下如果不用ES6，而用闭包如何解决这个问题。</p>
<pre class="hljs javascript"><code class="javascript"><span class="hljs-keyword">var</span> clickBoxs = <span class="hljs-built_in">document</span>.querySelectorAll(<span class="hljs-string">'.clickBox'</span>)
<span class="hljs-keyword">for</span> (<span class="hljs-keyword">var</span> i = <span class="hljs-number">0</span>; i &lt; clickBoxs.length; i++){
    clickBoxs[i].onclick = <span class="hljs-function"><span class="hljs-keyword">function</span>(<span class="hljs-params"></span>)</span>{
        <span class="hljs-built_in">console</span>.log(i)
    }
}</code></pre>
<p>我们本来希望的是点击不同的clickBox，显示不同的i，但事实是无论我们点击哪个clickBox，输出的都是5。下面我们来看下，如何用闭包搞定它。</p>
<pre class="hljs matlab"><code class="matlab"><span class="hljs-function"><span class="hljs-keyword">function</span> <span class="hljs-title">iteratorFactory</span><span class="hljs-params">(i)</span>{</span>
    var onclick = <span class="hljs-function"><span class="hljs-keyword">function</span><span class="hljs-params">(e)</span>{</span>
        console.<span class="hljs-built_in">log</span>(<span class="hljs-built_in">i</span>)
    }
    <span class="hljs-keyword">return</span> onclick;
}
var clickBoxs = document.querySelectorAll(<span class="hljs-string">'.clickBox'</span>)
<span class="hljs-keyword">for</span> (var <span class="hljs-built_in">i</span> = <span class="hljs-number">0</span>; <span class="hljs-built_in">i</span> &lt; clickBoxs.<span class="hljs-built_in">length</span>; <span class="hljs-built_in">i</span>++){
    clickBoxs[i].onclick = iteratorFactory(i)
}</code></pre>
<p><code>const</code>也用来声明变量，但是声明的是常量。一旦声明，常量的值就不能改变。</p>
<pre class="hljs monkey"><code class="monkey"><span class="hljs-keyword">const</span> <span class="hljs-built_in">PI</span> = Math.<span class="hljs-built_in">PI</span>

<span class="hljs-built_in">PI</span> = <span class="hljs-number">23</span> //<span class="hljs-keyword">Module</span> build failed: SyntaxError: /es6/app.js: <span class="hljs-string">"PI"</span> is read-only</code></pre>
<p>当我们尝试去改变用const声明的常量时，浏览器就会报错。<br>const有一个很好的应用场景，就是当我们引用第三方库的时声明的变量，用const来声明可以避免未来不小心重命名而导致出现bug：</p>
<pre class="hljs javascript"><code class="javascript"><span class="hljs-keyword">const</span> monent = <span class="hljs-built_in">require</span>(<span class="hljs-string">'moment'</span>)</code></pre>
<h3>class, extends, super</h3>
<p>这三个特性涉及了ES5中最令人头疼的的几个部分：原型、构造函数，继承...你还在为它们复杂难懂的语法而烦恼吗？你还在为指针到底指向哪里而纠结万分吗？</p>
<p>有了ES6我们不再烦恼！</p>
<p>ES6提供了更接近传统语言的写法，引入了Class（类）这个概念。新的class写法让对象原型的写法更加清晰、更像面向对象编程的语法，也更加通俗易懂。</p>
<pre class="hljs scala"><code class="scala"><span class="hljs-class"><span class="hljs-keyword">class</span> <span class="hljs-title">Animal</span> </span>{
    constructor(){
        <span class="hljs-keyword">this</span>.<span class="hljs-keyword">type</span> = <span class="hljs-symbol">'anima</span>l'
    }
    says(say){
        console.log(<span class="hljs-keyword">this</span>.<span class="hljs-keyword">type</span> + ' says ' + say)
    }
}

let animal = <span class="hljs-keyword">new</span> <span class="hljs-type">Animal</span>()
animal.says(<span class="hljs-symbol">'hell</span>o') <span class="hljs-comment">//animal says hello</span>

<span class="hljs-class"><span class="hljs-keyword">class</span> <span class="hljs-title">Cat</span> <span class="hljs-keyword">extends</span> <span class="hljs-title">Animal</span> </span>{
    constructor(){
        <span class="hljs-keyword">super</span>()
        <span class="hljs-keyword">this</span>.<span class="hljs-keyword">type</span> = <span class="hljs-symbol">'ca</span>t'
    }
}

let cat = <span class="hljs-keyword">new</span> <span class="hljs-type">Cat</span>()
cat.says(<span class="hljs-symbol">'hell</span>o') <span class="hljs-comment">//cat says hello</span></code></pre>
<p>上面代码首先用<code>class</code>定义了一个“类”，可以看到里面有一个<code>constructor</code>方法，这就是构造方法，而<code>this</code>关键字则代表实例对象。简单地说，<code>constructor</code>内定义的方法和属性是实例对象自己的，而<code>constructor</code>外定义的方法和属性则是所有实力对象可以共享的。</p>
<p>Class之间可以通过<code>extends</code>关键字实现继承，这比ES5的通过修改原型链实现继承，要清晰和方便很多。上面定义了一个Cat类，该类通过<code>extends</code>关键字，继承了Animal类的所有属性和方法。</p>
<p><code>super</code>关键字，它指代父类的实例（即父类的this对象）。子类必须在<code>constructor</code>方法中调用<code>super</code>方法，否则新建实例时会报错。这是因为子类没有自己的<code>this</code>对象，而是继承父类的<code>this</code>对象，然后对其进行加工。如果不调用<code>super</code>方法，子类就得不到<code>this</code>对象。</p>
<p>ES6的继承机制，实质是先创造父类的实例对象this（所以必须先调用super方法），然后再用子类的构造函数修改this。</p>
<p>P.S 如果你写react的话，就会发现以上三个东西在最新版React中出现得很多。创建的每个component都是一个继承<code>React.Component</code>的类。<a href="https://facebook.github.io/react/docs/reusable-components.html" target="_blank">详见react文档</a></p>
<h3>arrow function</h3>
<p>这个恐怕是ES6最最常用的一个新特性了，用它来写function比原来的写法要简洁清晰很多:</p>
<pre class="hljs php"><code class="php"><span class="hljs-function"><span class="hljs-keyword">function</span><span class="hljs-params">(i)</span></span>{ <span class="hljs-keyword">return</span> i + <span class="hljs-number">1</span>; } <span class="hljs-comment">//ES5</span>
(i) =&gt; i + <span class="hljs-number">1</span> <span class="hljs-comment">//ES6</span></code></pre>
<p>简直是简单的不像话对吧...<br>如果方程比较复杂，则需要用<code>{}</code>把代码包起来：</p>
<pre class="hljs lua"><code class="lua"><span class="hljs-function"><span class="hljs-keyword">function</span><span class="hljs-params">(x, y)</span></span> { 
    x++;
    y<span class="hljs-comment">--;</span>
    <span class="hljs-keyword">return</span> x + y;
}
(x, y) =&gt; {x++; y<span class="hljs-comment">--; return x+y}</span></code></pre>
<p>除了看上去更简洁以外，arrow function还有一项超级无敌的功能！<br>长期以来，JavaScript语言的<code>this</code>对象一直是一个令人头痛的问题，在对象方法中使用this，必须非常小心。例如：</p>
<pre class="hljs javascript"><code class="javascript"><span class="hljs-class"><span class="hljs-keyword">class</span> <span class="hljs-title">Animal</span> </span>{
    <span class="hljs-keyword">constructor</span>(){
        <span class="hljs-keyword">this</span>.type = <span class="hljs-string">'animal'</span>
    }
    says(say){
        setTimeout(<span class="hljs-function"><span class="hljs-keyword">function</span>(<span class="hljs-params"></span>)</span>{
            <span class="hljs-built_in">console</span>.log(<span class="hljs-keyword">this</span>.type + <span class="hljs-string">' says '</span> + say)
        }, <span class="hljs-number">1000</span>)
    }
}

 <span class="hljs-keyword">var</span> animal = <span class="hljs-keyword">new</span> Animal()
 animal.says(<span class="hljs-string">'hi'</span>)  <span class="hljs-comment">//undefined says hi</span></code></pre>
<p>运行上面的代码会报错，这是因为<code>setTimeout</code>中的<code>this</code>指向的是全局对象。所以为了让它能够正确的运行，传统的解决方法有两种：</p>
<ol>
<li>
<p>第一种是将this传给self,再用self来指代this</p>
<pre class="hljs javascript"><code class="javascript"> says(say){
     <span class="hljs-keyword">var</span> self = <span class="hljs-keyword">this</span>;
     setTimeout(<span class="hljs-function"><span class="hljs-keyword">function</span>(<span class="hljs-params"></span>)</span>{
         <span class="hljs-built_in">console</span>.log(self.type + <span class="hljs-string">' says '</span> + say)
     }, <span class="hljs-number">1000</span>)</code></pre>
<p>2.第二种方法是用<code>bind(this)</code>,即</p>
<pre class="hljs fortran"><code class="fortran"> says(say){
     setTimeout(<span class="hljs-function"><span class="hljs-keyword">function</span><span class="hljs-params">()</span></span>{
         console.<span class="hljs-built_in">log</span>(self.<span class="hljs-keyword">type</span> + <span class="hljs-string">' says '</span> + say)
     }.<span class="hljs-keyword">bind</span>(this), <span class="hljs-number">1000</span>)</code></pre>
<p>但现在我们有了箭头函数，就不需要这么麻烦了：</p>
</li>
</ol>
<pre class="hljs javascript"><code class="javascript"><span class="hljs-class"><span class="hljs-keyword">class</span> <span class="hljs-title">Animal</span> </span>{
    <span class="hljs-keyword">constructor</span>(){
        <span class="hljs-keyword">this</span>.type = <span class="hljs-string">'animal'</span>
    }
    says(say){
        setTimeout( <span class="hljs-function"><span class="hljs-params">()</span> =&gt;</span> {
            <span class="hljs-built_in">console</span>.log(<span class="hljs-keyword">this</span>.type + <span class="hljs-string">' says '</span> + say)
        }, <span class="hljs-number">1000</span>)
    }
}
 <span class="hljs-keyword">var</span> animal = <span class="hljs-keyword">new</span> Animal()
 animal.says(<span class="hljs-string">'hi'</span>)  <span class="hljs-comment">//animal says hi</span></code></pre>
<p>当我们使用箭头函数时，函数体内的this对象，就是定义时所在的对象，而不是使用时所在的对象。<br>并不是因为箭头函数内部有绑定this的机制，实际原因是箭头函数根本没有自己的this，它的this是继承外面的，因此内部的this就是外层代码块的this。</p>
<h3>template string</h3>
<p>这个东西也是非常有用，当我们要插入大段的html内容到文档中时，传统的写法非常麻烦，所以之前我们通常会引用一些模板工具库，比如mustache等等。</p>
<p>大家可以先看下面一段代码：</p>
<pre class="hljs smalltalk"><code class="smalltalk"><span class="hljs-string">$(</span><span class="hljs-comment">"#result"</span>).append(
  <span class="hljs-comment">"There are &lt;b&gt;"</span> + basket.count + <span class="hljs-comment">"&lt;/b&gt; "</span> +
  <span class="hljs-comment">"items in your basket, "</span> +
  <span class="hljs-comment">"&lt;em&gt;"</span> + basket.onSale +
  <span class="hljs-comment">"&lt;/em&gt; are on sale!"</span>
);</code></pre>
<p>我们要用一堆的'+'号来连接文本与变量，而使用ES6的新特性模板字符串``后，我们可以直接这么来写：</p>
<pre class="hljs stata"><code class="stata">$(<span class="hljs-string">"#result"</span>).<span class="hljs-keyword">append</span>(`
  There are &lt;b&gt;<span class="hljs-variable">${basket</span>.<span class="hljs-keyword">count</span>}&lt;/b&gt; items
   <span class="hljs-keyword">in</span> your basket, &lt;em&gt;<span class="hljs-variable">${basket</span>.onSale}&lt;/em&gt;
  are <span class="hljs-keyword">on</span> sale!
`);</code></pre>
<p>用反引号<code>（`）</code>来标识起始，用<code>${}</code>来引用变量，而且所有的空格和缩进都会被保留在输出之中，是不是非常爽？！</p>
<p>React Router从第1.0.3版开始也使用ES6语法了，比如这个例子：<br><code>&lt;Link to={`/taco/${taco.name}`}&gt;{taco.name}&lt;/Link&gt;</code><br><a href="https://github.com/rackt/react-router/blob/latest/examples/passing-props-to-children/app.js" target="_blank">React Router</a></p>
<h3>destructuring</h3>
<p>ES6允许按照一定模式，从数组和对象中提取值，对变量进行赋值，这被称为解构（Destructuring）。</p>
<p>看下面的例子：</p>
<pre class="hljs vim"><code class="vim"><span class="hljs-keyword">let</span> <span class="hljs-keyword">cat</span> = <span class="hljs-string">'ken'</span>
<span class="hljs-keyword">let</span> dog = <span class="hljs-string">'lili'</span>
<span class="hljs-keyword">let</span> zoo = {<span class="hljs-keyword">ca</span><span class="hljs-variable">t:</span> <span class="hljs-keyword">cat</span>, <span class="hljs-keyword">do</span><span class="hljs-variable">g:</span> dog}
console.<span class="hljs-built_in">log</span>(zoo)  //Object {<span class="hljs-keyword">ca</span><span class="hljs-variable">t:</span> <span class="hljs-string">"ken"</span>, <span class="hljs-keyword">do</span><span class="hljs-variable">g:</span> <span class="hljs-string">"lili"</span>}</code></pre>
<p>用ES6完全可以像下面这么写：</p>
<pre class="hljs vim"><code class="vim"><span class="hljs-keyword">let</span> <span class="hljs-keyword">cat</span> = <span class="hljs-string">'ken'</span>
<span class="hljs-keyword">let</span> dog = <span class="hljs-string">'lili'</span>
<span class="hljs-keyword">let</span> zoo = {<span class="hljs-keyword">cat</span>, dog}
console.<span class="hljs-built_in">log</span>(zoo)  //Object {<span class="hljs-keyword">ca</span><span class="hljs-variable">t:</span> <span class="hljs-string">"ken"</span>, <span class="hljs-keyword">do</span><span class="hljs-variable">g:</span> <span class="hljs-string">"lili"</span>}</code></pre>
<p>反过来可以这么写：</p>
<pre class="hljs fsharp"><code class="fsharp"><span class="hljs-keyword">let</span> dog = {<span class="hljs-class"><span class="hljs-keyword">type</span>: '<span class="hljs-title">animal</span>', <span class="hljs-title">many</span>: 2}</span>
<span class="hljs-keyword">let</span> { <span class="hljs-class"><span class="hljs-keyword">type</span>, <span class="hljs-title">many</span>} </span>= dog
console.log(<span class="hljs-class"><span class="hljs-keyword">type</span>, <span class="hljs-title">many</span>)   //<span class="hljs-title">animal</span> 2</span></code></pre>
<h3>default, rest</h3>
<p>default很简单，意思就是默认值。大家可以看下面的例子，调用<code>animal()</code>方法时忘了传参数，传统的做法就是加上这一句<code>type = type || 'cat'</code>来指定默认值。</p>
<pre class="hljs fsharp"><code class="fsharp"><span class="hljs-keyword">function</span> animal(<span class="hljs-class"><span class="hljs-keyword">type</span>){</span>
    <span class="hljs-class"><span class="hljs-keyword">type</span> </span>= <span class="hljs-class"><span class="hljs-keyword">type</span> || '<span class="hljs-title">cat</span>'  </span>
    console.log(<span class="hljs-class"><span class="hljs-keyword">type</span>)</span>
}
animal()</code></pre>
<p>如果用ES6我们而已直接这么写：</p>
<pre class="hljs delphi"><code class="delphi"><span class="hljs-function"><span class="hljs-keyword">function</span> <span class="hljs-title">animal</span><span class="hljs-params">(<span class="hljs-keyword">type</span> = <span class="hljs-string">'cat'</span>)</span><span class="hljs-comment">{
    console.log(type)
}</span>
<span class="hljs-title">animal</span><span class="hljs-params">()</span></span></code></pre>
<p>最后一个rest语法也很简单，直接看例子：</p>
<pre class="hljs actionscript"><code class="actionscript"><span class="hljs-function"><span class="hljs-keyword">function</span> <span class="hljs-title">animals</span><span class="hljs-params">(<span class="hljs-rest_arg">...types</span>)</span></span>{
    console.log(types)
}
animals(<span class="hljs-string">'cat'</span>, <span class="hljs-string">'dog'</span>, <span class="hljs-string">'fish'</span>) <span class="hljs-comment">//["cat", "dog", "fish"]</span></code></pre>
<p>而如果不用ES6的话，我们则得使用ES5的<code>arguments</code>。</p>
<h3>总结</h3>
<p>以上就是ES6最常用的一些语法，可以说这20%的语法，在ES6的日常使用中占了80%...</p>

<h3>文章来源</h3>
<p>http://www.jianshu.com/p/ebfeb687eb70</p>

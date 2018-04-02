# RxJavaOperatorAndMap

<h1 id="introducingoperators">Introducing Operators in RxJava tutorial for beginners</h1>
<img src="http://thoughtnerds.com/wp-content/uploads/2018/03/1_HDxfd3bSPwgdQ9A3TthVYA-300x143.png" alt="" width="721" height="344" class=" wp-image-886 aligncenter" />

Checkout the previous post <a href="http://thoughtnerds.com/transformation-using-rxjava-tutorial-beginners/">here</a>

Here's how we're going to solve the item transformation problems: with operators. Operators can be used in between the source<span> </span><code>Observable</code><span> </span>and the ultimate<span> </span><code>Subscriber</code><span> </span>to manipulate emitted items. RxJava comes with a huge collection of operators, but its best to focus on just a handful at first.

For this situation, the<span> </span><code>map()</code><span> </span>operator can be used to transform one emitted item into another:
<pre><code>Observable.just("Hello, world!")
    .map(new Func1&lt;String, String&gt;() {
        @Override
        public String call(String s) {
            return s + " -Dan";
        }
    })
    .subscribe(s -&gt; System.out.println(s));
</code></pre>
Again, we can simplify this by using lambdas:
<pre><code>Observable.just("Hello, world!")
    .map(s -&gt; s + " -Dan")
    .subscribe(s -&gt; System.out.println(s));
</code></pre>
Pretty cool, eh? Our<span> </span><code>map()</code><span> </span>operator is basically an<span> </span><code>Observable</code><span> </span>that transforms an item. We can chain as many<span> </span><code>map()</code><span> </span>calls as we want together, polishing the data into a perfect, consumable form for our end<span> </span><code>Subscriber</code>.

&nbsp;

[amazon_link asins='9352134664,B0784QQV4W,B073L5JB27,1484214293,B071VG1VY9,1787120422,1784399108' template='ProductCarousel' store='200' marketplace='IN' link_id='71151eab-363b-11e8-a2a4-cb611b633f41']
<h1 id="moreonmap">More on map()</h1>
Here's an interesting aspect of<span> </span><code>map()</code>: it does not have to emit items of the same type as the source<span> </span><code>Observable</code>!

Suppose my<span> </span><code>Subscriber</code><span> </span>is not interested in outputting the original text, but instead wants to output the hash of the text:
<pre><code>Observable.just("Hello, world!")
    .map(new Func1&lt;String, Integer&gt;() {
        @Override
        public Integer call(String s) {
            return s.hashCode();
        }
    })
    .subscribe(i -&gt; System.out.println(Integer.toString(i)));
</code></pre>
Interesting - we started with a String but our<span> </span><code>Subscriber</code><span> </span>receives an Integer. Again, we can use lambdas to shorten this code:
<pre><code>Observable.just("Hello, world!")
    .map(s -&gt; s.hashCode())
    .subscribe(i -&gt; System.out.println(Integer.toString(i)));
</code></pre>
Like I said before, we want our<span> </span><code>Subscriber</code><span> </span>to do as little as possible. Let's throw in another<span> </span><code>map()</code><span> </span>to convert our hash back into a String:
<pre><code>Observable.just("Hello, world!")
    .map(s -&gt; s.hashCode())
    .map(i -&gt; Integer.toString(i))
    .subscribe(s -&gt; System.out.println(s));
</code></pre>
Would you look at that - our<span> </span><code>Observable</code><span> </span>and<span> </span><code>Subscriber</code><span> </span>are back to their original code! We just added some transformational steps in between. We could even add my signature transformation back in as well:
<pre><code>Observable.just("Hello, world!")
    .map(s -&gt; s + " -Dan")
    .map(s -&gt; s.hashCode())
    .map(i -&gt; Integer.toString(i))
    .subscribe(s -&gt; System.out.println(s));
</code></pre>

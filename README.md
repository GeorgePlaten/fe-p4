<h2>Website Performance Optimization portfolio project</h2>

<p>This is my second run-through of this project, the first was done very
manually and as part of a progressive learning experience. I decided
I would like to redo the project from the start while applying the tools
and techniques I have learned on the first run-through.</p>

<p>In particular, I discovered the concept of using build tools and from
there I found Google's <a
href="https://developers.google.com/web/starter-kit/">Web Starter Kit</a>
(WSK), which I have decided to use this time.</p>


<h3>Getting started</h3>
<p>Steps taken to get started:</p>
<ol>
  <li>Clone the WSK repository</li>
  <li>Clone the project repository into WSK.</li>
  <li>Refactor project code to work with WSK file and directory structure.</li>
  <li>Remove unwanted and unneeded utilities from WSK.</li>
  <li>Edit and test gulpfile.js to serve and build/serve </li>
</ol>

<h3>Part 1: Optimize PageSpeed Insights (PSI) score for index.html</h3>
<p>Steps taken to achieve PSI scores of 90+:</p>

<ol>
  <li>Refactor project to be built with WSK for automatic Code Minification
  and Image Optimization.<br>
  <em>From: Mobile/Desktop 60/67 To: 61/68 <strong>Change:
  +1/+1</strong></em></li>
  <li>Optimize Images
    <ol>
      <li>profile.jpg: replace file with optimized version as provided by
      PSI</li>
      <li>pizzeria.jpg:  hoist the optimized copy provided by PSI to the
      root images directory, further resize the image to allow it be served
      at natural dimensions.</li>
      <li>Content section thumbnails: use local copies and fix CSS to serve
      images at natural dimensions.</li>
      <li>All images: add height and width attributes</li>
    </ol>
    <em>From: Mobile/Desktop 61/68 To: 76/89 <strong>Change:
    +15/+21</strong></em>
  </li>
  <li>Eliminate render-blocking JavaScript and CSS in above-the-fold
  content:
    <ol>
      <li>Add media attribute to print stylesheet, to allow it to load
      asynchronously.</li>
      <li>Inline the Webfont in the CSS file.</li>
      <li>Inline the CSS (minified) in the HTML</li>
      <li>Inline perfmatters.js (minified) in the HTML, before CSS to ensure it
      is not render blocking. This JavaScript could be loaded asynchronously
      but it is small enough to be inlined in the head.</li>
      <li>Remove Google Analytics scripts. Again they could be async'd, if they
      were needed.</li>
      <li>Remove unnecessary meta tags from head.</li>
    </ol>
  </li>
  <em>From: Mobile/Desktop 76/89 To: 95/96 <strong>Change: +19/+7</strong></em>
  <p><strong>Final PageSpeed Results for index.html: Mobile/Desktop
  95/96</strong></p>
</ol>

<h3>Part 2: Optimize Frames per Second in pizza.html</h3>

<ol>
  <li>
    Image Optimization
    <ol>
      <li>Replace pizzeria.jpg with PSI optimized resource</li>
      <li>Downsample pizza.png to 35 color palette (10 kb)</li>
      <li>Create smaller version of pizza.png for the background without
      transparency and to be served at natural size.</li>
    </ol>
  </li>
  <li>
    Optimize HTML, CSS and JavaScript with WSK and gulp
    <ol>
      <li>Use <em>uglify</em> for JavaScript</li>
      <li>Use <em>csso</em> to optimize and minify CSS</li>
      <li>Use <em>minify-html</em> to minify pizza.html</li>
    </ol>
  </li>
  <li>
    Eliminate render-blocking JavaScript and CSS in above-the-fold content
    <ol>
      <li>Use <em>uncss</em> to remove unused bootstrap styles</li>
      <li>Inline all CSS into head of pizza.html and JavaScript into body.</li>
      <li>Add device-with content meta tag to assist consistent layout</li>
    </ol>
  </li>
  <li>
    <strong>main.js</strong>: Fix Layout Thrashing caused by updatePositions()
    <ol>
      <li>Hoist <code>phase</code> invariant from for loop.</li>
      <li>Hoist <code>items = document.querySelectorAll('.mover')</code> query
      from function scope to global scope so it is not called with each scroll
      event. Rename <code>items</code> to <code>movingPizzas</code> as it need
      to make more sense in global context. <code>movingPizzas</code> is then
      set during the <code>DOMContentLoaded</code> event after all the pizzas
      are added.</li>
      <li>Cache length of the array in the loop declaration.</li>
      <li>Remove <code>document.body.scrollTop</code> layout tiggering query
      to new intermediate calling <code>scroller()</code> function and pass it
      as parameter <code>shift</code>.</li>
      <li>Make <code>shift</code> a global variable with default value of 5
      to prevent layout thrashing on first page load.</li>
      <li>Use CSS <code>transform: translateX()</code> property to move the
      pizza divs instead of <code>left</code> property for improved performance.
    </ol>
  </li>
  <li>
    <strong>main.js</strong>: Improve painting performance while scrolling
    <ol>
      <li>Dynamically set the number of background pizzas based on window size
      during <code>DOMContentLoaded</code> event, as painting performance scales
      with element quantity. This also requires an event listener to goldilocks
      the pizza count when the browser window is resized.</li>
      <li>Add <code>backface-visibility: hidden</code> hack to CSS for mover
      class, and <code>will-change: transform</code> hint for browsers that can
      use it.</li>
    </ol>
  </li>
  <li><strong>main.js</strong>: Hoist invariant from Random Pizza generator
  loop</li>
  <li>
    <strong>main.js</strong>: Optimize resizePizzas function
    <ol>
      <li>Hoist invariants from functions changePizzaSizes.</li>
      <li>Change determineDx function to return simple percentage value and
      assignment for use in changePizzaSizes.</li>
      <li>Use a style change to change all pizzas at once instead of looping one
      by one. (determineDx function not now required).</li>
    </ol>
  </li>
</ol>
<p><strong>Final PageSpeed Results for pizza.html: Mobile/Desktop
98/98</strong></p>
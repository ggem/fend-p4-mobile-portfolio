## Website Performance Optimization Portfolio Project ##

This project consists in optimizing an online portfolio for speed.

You can find this code at <https://github.com/ggem/fend-p4-mobile-portfolio>, and you can see a running version at <http://ggem.github.io/fend-p4-mobile-portfolio/>.

### Part 1: Optimize PageSpeed Insights Score for index.html ###
1. Scaling pizzeria.jpg image.
    * [pizzeria.jpg](http://ggem.github.io/fend-p4-mobile-portfolio/views/images/pizzeria.jpg) is a 2048x1536 image using 2.3MB.
    * [index.html](http://ggem.github.io/fend-p4-mobile-portfolio/index.html) uses the image scaled down to 100x75.
    * Storing the scaled down image in [pizzeria-100x75.png](http://ggem.github.io/fend-p4-mobile-portfolio/views/images/pizzeria-100x75.png) results in a PageInsights Score improvement from 28 to 76 in mobile and from 30 to 89 in desktop.
    * Github commit: [7b6b9789](https://github.com/ggem/fend-p4-mobile-portfolio/commit/7b6b9789f716c9da6e2cb9370c020eb3cc6da084)
1. Using loadCSS to load CSS files asynchronously.
    * As mentioned in [piazza.com](https://piazza.com/class/i0sf6tsmg0r7do?cid=1074), there is a function that loads CSS synchronously, aptly named [loadCSS](https://github.com/filamentgroup/loadCSS/).
    * Using loadCSS, the score goes from 76 to 93 in mobile and from 89 to 91 in desktop.
    * Github commit: [45fc9bf2](https://github.com/ggem/fend-p4-mobile-portfolio/commit/45fc9bf2e543dcf4fd94f40bf978e28794730c6f)
1. Automatically minimizing HTML/JS/CSS files.
    * WebStorm has [File Watchers](https://www.jetbrains.com/webstorm/help/file-watchers.html), that performs any specified operation when a file has been modified.  This can be used to automatically minimize HTML, Javascript and CSS files.
    * HTML files are minimized with [htmlcompressor](https://code.google.com/p/htmlcompressor/).
    * Javascript files are minimized with [Closure Compiler](https://developers.google.com/closure/).
    * CSS files are minimized with [yuicompressor](http://yui.github.io/yuicompressor/)
    * Minimizing files, the score goes from 93 to 94 for mobile and from 91 to 93 for desktop.
    * Github commits: [012a7596](https://github.com/ggem/fend-p4-mobile-portfolio/commit/012a75969bcbab755ab4badbc506ba3d8c30d0b6) and [dafd954a](https://github.com/ggem/fend-p4-mobile-portfolio/commit/dafd954a10fd0125cef17c2b472ce7a21fcb3d4f)

### Part 2: Optimize Frames Per Second in pizza.html ###
1. Computing new pizza width only once.
    * All the pizzas are the same size, so we need to compute the new size only once, and not 100 times (once per pizza).
    * Also, `document.querySelectorAll(".randomPizzaContainer")` doesn't need to be computed 400 times as in the original code.  Only once is enough.
    * With this changes, the time to resize the pizzas goes from 158 ms to 1.3 ms (in my laptop).
    * Github commit: [e092f982](https://github.com/ggem/fend-p4-mobile-portfolio/commit/e092f98282e6ab904e0153f7eed326e278221cdd)
1. Moving phase computing outside the loop.
    * When updating the positions, there are only 5 different phases (the phase depends on `document.body.scrollTop`, which is a constant in the loop, and on `i % 5`, which has only 5 possible values).
    * By doing the computation outside the loop, we do them only 5 times, instead of 200.  The average frame time goes from 40 ms to 0.7 ms in my laptop.
    * Github commit: [434a43d0](https://github.com/ggem/fend-p4-mobile-portfolio/commit/434a43d04f8cf27397c35a615c6b3daefb8da256).

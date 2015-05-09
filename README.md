# Optimizations in project

##60 fps

### Scrolling optimizations
1.   Pizza horizontal offset depends only on column where it located. So, we can precalculates these offsets and reuse them further.
2.   Only couple of pizzas are visible on screen. I can reduce number of them, but decided to make more general approach. 
     First, I tried to use HTMLElement.offset function, but it leads to layout recalculation which shouldn't be done in cycle.
3.   document.getElementsByClassName is faster then document.querySelectorAll
4.   transform : translateX (instead of changing left property) could be processed on GPU and requires only compositioning (can reduce number of paints).
5.   Set will-change property instead of translateZ hack.

### Change size optimizations
1. Removed ineffective code duplication (multiple calls of querySelectorAll(".randomPizzaContainer"))
2. Removed unnecessary determineDx function
3. Document.getElementsByClassName is faster then document.querySelectorAll

### Other optimizations
Improved page load time by putting all new pizzas in container and attaching it to DOM only once.


### Problems
60 fps: Still got large composite layers time (sometimes 20-30 ms). May be it is real problem?  
        Document size is large (941x12223px) and there are many layers (.mover objects)?
        Have no idea how I can reduce it.
       
##CRP Optimizations
1. Moved GA-related scripts to end of page
2. Got rid of web fonts
3. Set media=print for print.css to avoid blocing page load for print-only styles
4. Reduced size of pizzeria.jpg image
5. Inlined some css (not looks like good production approach, but can't reach score without it)
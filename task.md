# **Technical paper on How to Render HTML, CSS, JavaScript to Dom**.

## **Written by Prince Kumar**

## **Table of Content**
 1. ### **How to render html to dom**
 1. ### **How to render css to dom**
 1. ### **How to render JavaaScript to dom** 
 1. ### **References**
---
1. ## **How to render html to dom.**

First, the **raw bytes** are converted into **characters**. This conversion is done on the character encoding of the **HTML** file. These character are futher passed into token which is a bunch of character in a test file. HTML code does not produce an actual website, bwhen we save the file with .html extension, the browser signals engine to interpret file as HTML document.
During this the parse understand each string in angle bracket and understand set of rules that apply to each of them. Essentially an HTML file is broken into small unit of parsing called token that browser understand what we have written. Futher token converted into node, these nodes are futher linked in a tree data structure known as DOM. It is a child parent, adjacent relationship etc. A web design does not open in CSS and JS file, We generally open HTML file in index.html. This is exactly why we do so:the browser must go through transforming the raw byte of HTML data into the DOM before anything an happen, Depending upon how long the html file is, the DOM  construction process may take some time. No matter how small, it does takev time, regardless of the file size.


                     Bytes--->Characters--->Tokens--->Node--->DOM

2. ## **How to render CSS to DOM.**

There is CSS tree structure called the CSS Object Model (CSSOM), We know that the browser can't with either raw bytes of HTML or CSS. This has to be converted to a form it recognize and that happens to be these tree structure.


       CSS Bytes--->Characters--->Tokens--->Node--->CSSOM

CSS has something called the **cascade**. The cascade how the browser determines what styles are applied to an element. Because styles affecting an element may come from a parent element or have been set on the element themselves, the CSSOM tree structure become important, this is because the browser has to recursively go through the CSS tree structure and determine the style that affect a particular element.

# **The Render Tree**
The DOM and CSSOM tree structure are two independent structures. The DOM contains qall the information about the page's HTML element's ralationships, while the CSSOM contain information on how the elements are styled, the browser now combine the DOM and CSSOM trees into something called a render tree.
              
              DOM + CSS = Render tree
The render tree contain information on all visible DOM content on page and the required CSSOM information for the different nodes. The hidden elements will be present in the DOM but not the render  tree combine, this is because the render tree combine from both the DOM and the CSSOM.

---
3. ## **How to render JavaScript to DOM.**

By implication javaScript we can remove and add element from the DOM tree and we can modify the CSSOM property of an element via a javascript as well.The simplw HTML and CSS code we can render simple text ann image on the screen from previous explanation the browser reads raw byte of the HTML file from the disk(or network) and transform that into characters which further parsed into tokens. When the parser find any css file  eg. style.css, a request is made to fetch the CSS file, style.css the DOM construction continues, and as soon as this css file returns with some content, the CSSOM construction begin.

But whenever the browser encounters a script tag the DOM construction is paused, entire DOM construction process is haulted untill the script finishes executing then this is because javascript can alter both DOM and CSSOM. Because browser is not sure what this particular JavaScript will do, It take precations by haulting the entire DOM construction all together.

The location of script also matter where we have described it in a code. If this script tag is written at the bottom of the code and try to access the DOM for a node with ID and Header and then login it to the console this will work fine, If we place it in the head tag the the header variable is resolved to null. This is because while the html parser was in the process of constructing the DOM, a script tag was found at this time body tag and all its content had not been parsed, The DOM construction is haulted untill the script execution is complete, By the time the script attemted to accesss the DOM node with an id of Header, It does not exits because the Dom had not finished parsing the document.
Similarly if we try to extract the inline script to an external local file the behaviour is just the same i.e DOM construction is still hault. If we try to fetch this external local file over the internet and if the internet is slow then it takes thousands of mili seconds to fetch file, The DOM construction will be haulted for the thoudand of mili seconds as well which leads to big performance issue.
Remember that javascript can also the CSSOM and make changes to it. So ,what happen to the when the parser encounters a script tag but the CSSOM is not redy yet? well  the answer is the javascript execution will be haulted untill the CSSOM is ready, so eventhough the DOM construction stop untill an encountered script tag is encountered that is not happen with the CSSOM. With the CSSOM the JS execution and with no CSSOM no js execution, By default every script is a parser blocker! the DOM construction will always be haulted but if we add the "async" keyword to the script tag the dOM construction will not be haulted. The DOM construction will be continued and the script will be continue when it is done downloading and ready.  

#  **Reference Link**

- https://blog.logrocket.com/how-browser-rendering-works-behind-scenes/
- https://developers.google.com/web/fundamentals/performance/critical-rendering-path/render-tree-construction
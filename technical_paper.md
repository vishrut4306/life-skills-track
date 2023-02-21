# Browser Rendering of HTML, CSS, JS to DOM and its meachanism.

## Introduction

In this paper, we'll investigate activities performed by a program to deliver a page. Steps engaged with HTML page delivery:

- Development of DOM
- Development of CSSOM
- Development of Render tree
- Format Stage
- Painting Stage
<hr>

## Development of DOM

![](https://media.geeksforgeeks.org/wp-content/uploads/20210908120846/DOM.png)

```
<!DOCTYPE html>
<html>
  <head>
    <meta name="viewport" content="width=device-width,initial-scale=1" />
    <link href="style.css" rel="stylesheet" />
    <title>Critical Path</title>
  </head>
  <body>
    <p>Hello <span>web performance</span> students!</p>
    <div><img src="awesome-photo.jpg" /></div>
  </body>
</html>
```

We should begin with the most straightforward case: a plain HTML page with a few text and a solitary picture. How does the program interact with this page?

**Conversion.** The browser reads the raw bytes of HTML off the disk or network, and translates them to individual characters based on specified encoding of the file (for example, UTF-8).

**Tokenizing.** The browser converts strings of characters into distinct tokens—as specified by the W3C HTML5 standard for example, "<html>", "<body>"—and other strings within angle brackets. Each token has a special meaning and its own set of rules.

**Lexing.** The emitted tokens are converted into "objects," which define their properties and rules.

**DOM construction.** Finally, because the HTML markup defines relationships between different tags (some tags are contained within other tags) the created objects are linked in a tree data structure that also captures the parent-child relationships defined in the original markup: the HTML object is a parent of the body object, the body is a parent of the paragraph object, and so on.

<hr>

- The program gets an HTML report from the server in the binary stream design, which is fundamentally a text document with a reaction header Content-Type = text/html; charset=UTF-8.
- At the point when the program peruses the HTML record, at whatever point it experiences an HTML component, it makes a JS object called a Hub. In the end, all HTML components will be switched over completely to a Hub.
- After the program has made hubs from the HTML record, it needs to make a "tree-like" construction of these hub objects.
<hr>

## DOM Summary

- Bytes → characters → tokens → nodes → object model.
- HTML markup is changed into a Report Item Model (DOM); CSS markup is changed into a CSS Article Model (CSSOM).
- DOM and CSSOM are independent data structures.
- Chrome DevTools Performance panel allows us to capture and inspect the construction and processing costs of DOM and CSSOM.
<hr>

## Development of CSSOM

![](https://i.stack.imgur.com/bsiD6.png)

While the program was building the DOM of our basic page, it experienced a connection label in the head part of the report referring to an outer CSS template: style.css. Guessing that it needs this asset to deliver the page, it quickly dispatches a solicitation for this asset, which returns with the accompanying substance:

```
body {
  font-size: 16px;
}
p {
  font-weight: bold;
}
span {
  color: red;
}
p span {
  display: none;
}
img {
  float: right;
}
```

We could have declared our styles directly within the HTML markup (inline), but keeping our CSS independent of HTML allows us to treat content and design as separate concerns: designers can work on CSS, developers can focus on HTML, and so on.

As with HTML, we need to convert the received CSS rules into something that the browser can understand and work with. The CSS bytes are converted into characters, then tokens, then nodes, and finally they are linked into a tree structure known as the "CSS Object Model" (CSSOM):

- In the wake of building the DOM, the program peruses CSS from every one of the sources and develops a CSSOM (CSS Item Model) - a tree-like construction.
- Every hub in this tree contains CSS-style data that will be duplicated to the DOM component it targets.
- The majority of the program accompanies its template which is called the client specialist template. It is a default template utilized by internet browsers. without any CSS applied, the program actually needs to deliver the substance in some way or another, and the program utilizes the client specialist template for that.
<hr>

## CSSOM Summary

The CSSOM and DOM trees are joined into a render tree, which is then used to register the format of each noticeable component and fills in as a contribution to the paint cycle that delivers the pixels to screen. Improving every one of these means is basic to accomplishing ideal delivering execution.

In the past segment on developing the item model, we fabricated the DOM and the CSSOM trees in light of the HTML and CSS input. Notwithstanding, both of these are autonomous items that catch various parts of the archive: one portrays the substance, and the other depicts the style decides that should be applied to the report.

- The DOM and CSSOM trees combine to form the render tree.
- Render tree contains just the nodes expected to deliver the page.
- Layout computes the exact position and size of each object.
- The last step is paint, which takes in the final render tree and renders the pixels to the screen.
<hr>

## Development of Render Tree

![](https://web-dev.imgix.net/image/C47gYyWYVMMhDmtYSLOWazuyePF2/b6Z2Gu6UD1x1imOu1tJV.png?auto=format&w=1600)

- DOM and CSSOM are joined together to frame a Render tree that contains the hubs which must be shown on the page.
- From the foundation of the DOM tree, each noticeable hub is navigated and an individual CSSOM rule is applied. At last, it gives the render tree containing apparent hubs with content and styling.
- It is a low-level portrayal of what will ultimately get imprinted on the screen, it will not contain hubs that hold no region in the pixel lattice (like head, meta, or connect labels).
<hr>

## Layout Phase

- This stage can be said as a math stage, where we compute the calculation of the hubs.
- In the design stage, the specific place of the hubs and their size separate from the view-port of the program is processed. Along these lines, a container model is produced which knows the specific positions and size. This cycle is otherwise called design or reflow.
- Box model generated in the Layout phase:
<hr>

## Painting Phase

- As we probably are aware of the apparent hubs, their styling, and their math, presently this data is utilized to deliver the hubs from the render tree to genuine pixels on the screen. This cycle is alluded to as Painting. It utilizes the UI backend layer.
<hr>

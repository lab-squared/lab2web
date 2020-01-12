# lab2web
### lab2web builds webs (mental maps) presenting current research.
Read more and view an example on [labSquared](https://lab-squared.github.io/), at [lab-squared.github.io/metabolism/](https://lab-squared.github.io/metabolism/o)

Made by Pablo Cárdenas, [pablo-cardenas.com](pablo-cardenas.com)

Based on [JSNetworkX](http://jsnetworkx.org). Colors taken from [this color blind palette](http://www.cookbook-r.com/Graphs/Colors_(ggplot2)/),
which was developed by [Okabe & Ito](http://jfly.iam.u-tokyo.ac.jp/color/).

# Rationale
Nodes on the map contain lines of research, with text color denoting research hypotheses or methodological/technology development. The color of a node denotes a hypothesis or technology's status as supported by evidence, disproven, or lacking enough evidence for a conclusion. Finally the border type signals a node's status as archived, current, or future line of research. Clicking on a node can present the evidence for or against the hypothesis or technology according to the lab's research, and link directly to the data and methods, protocols, and code used to acquire and analyze it. Data, protocols, and code are stored on a public, version-controlled repository such as GitHub, which allows easy tracking of where and when changes were made. Nodes are connected by directed edges, symbolizing logical or procedural dependence between different hypotheses and technologies. Tightly connected clusters of nodes can signal a unit publishable as a research article.

## Dependencies and installation
lab2web is built on JSNetworkX, which itself requires the [D3 visualization library](https://d3js.org/) (specifically version 3, not 4). However, lab2web requires a slightly modified version of JSNetworkX (JSNetworkX_lab2web) with clickable graph nodes. [You can view that fork of JSNetworkX on GitHub](https://github.com/lab-squared/JSNetworkX/tree/master).

To load JSNetworkX_lab2web, D3, and lab2web itself, include the `jsnetworkx_lab2web.js` and `lab2web.js` files on your website directory. Then, add the following in your HTML header:

```html
<script src="http://d3js.org/d3.v3.min.js" charset="utf-8"></script>
<script type="text/javascript" src="./jsnetworkx_lab2web.js"></script>
<script type="text/javascript" src="./lab2web.js"></script>
```
Be sure the file paths point to the correct locations of your copies of `jsnetworkx_lab2web.js` and `lab2web.js`.

## Usage
Next, let's build our research web. Besides the above scripts, lab2web needs three things:

- an input file with the contents and structure of your research web
- a `div` element on which to draw our research web
- a function call that draws the input file contents inside the given `div` element

Optionally, you can include a CSS file to be used when a node is clicked to display information (you can use the `lab2web.css` file included here).

### Input file format
The input file is a text file containing information for all the nodes on the network, which are different lines of research. Each research node should contain the all following information, strictly in the order shown:

```
ID: identifier (cannot contain commas or line breaks)
TITLE: title of line of research (cannot contain line breaks)
TYPE: HYPOTHESIS/METHODOLOGY (must be one of the two, in all caps)
STATUS: SUPPORTED/DISPROVEN/UNCLEAR (must be one of the three, in all caps)
ACTIVITY: ARCHIVED/CURRENT/FUTURE (must be one of the three, in all caps)
DATA: Information to be shown on click. Can contain commas, line breaks, markdown, HTML
METHODS: Information to be shown on click. Can contain commas, line breaks, markdown, HTML
CONNECTIONS: list of identifiers for other research nodes this node is connected to, separated by commas (no spaces)
```

View the `sample_input.txt` for an example.

If you want to link nodes to specific URLs instead of having them display information from the input file, you can do so by starting the `DATA:` field with the keyword `OPEN:` and then pasting the URL you wish to link to, as follows:
```
DATA: OPEN: https://www.mit.edu/~xela/tao.html
```

## Drawing the research web

To draw the research web, we need a `div` to locate the research web on the website and a function call to `lab2web('file_path_to_input.txt','file_path_to_stylesheet.css','div_id')`, the main method that renders the research web. To do both things, include the following HTML wherever you want to place the research web:

```html
<div id="lab2web"></div>
<script type="text/javascript">
  lab2web('./sample_input.txt', './lab2webStyle.css', 'lab2web');
</script>
```
Be sure the file paths point to the correct locations and names of your input and CSS files, and make sure the ID of your div matches the one you pass to the `lab2web()` function.

Additional options allow you to specify the shape of nodes as rectangles (`'rect'`, default) or ellipses (`'ellipse'`), to allow nodes to stay fixed in place when dragged (`true` by default), specify the dimensions of the canvas where the web is drawn (defaults to `null`, which results in the dimensions of the container being used), and show or hide the legend (defaults to `true`).

```javascript
lab2web('./sample_input.txt', '../lab2webStyle.css', 'lab2web', 'rect', false, null, 500, true);
```

## Testing
Running lab2web locally can result in a `Cross-Origin Request Blocked` error, in which the browser blocks lab2web from loading the input file contents because they are not coming from a secure server with an `https` domain. To successfully test it locally, set up a local server (I used [mkdocs](https://www.mkdocs.org/)) or deactivate this error manually on your browser.

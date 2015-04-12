viewport.js
===========


`viewport.js` is a small javascript library which ships the document
sections with additional properties containing the viewport scrolling
position relatively to the sections. Using these properties you can
create a custom scrolling indicator or a navigation menu precisely
reflecting the scrolling state:

[Demo](http://asvd.github.io/viewport) / [and its
source](https://github.com/asvd/asvd.github.io/tree/master/viewport)

In other words, `viewport.js` is similar to
[other](http://davidwalsh.name/js/scrollspy)
[scrollspy](http://getbootstrap.com/javascript/#scrollspy) [solutions]
(https://github.com/sxalexander/jquery-scrollspy), and has the
following advantages:

- it is written on vanilla javascript, does not have dependencies and
  works anywhere;

- its size is only 1581 bytes minified;

- it has a simple and flexible API which shows:

 - which section is currently displayed in the viewport;

 - where is the viewport relatively to each section;

 - where are the viewport edges relatively to each section;

 - where should the viewport be scrolled to in order to show a
   particular section.


### Usage

Download the
[distribution](https://github.com/asvd/viewport/releases/download/v0.0.1/viewport-0.0.1.tar.gz),
unpack it and load the `viewport.js` in a preferable way (that is an
UMD module):

```html
<script src="viewport.js"></script>
```

Add the `section` class to the sections:

```html
<div id=firstSection class=section>
    First section content goes here...
</div>

<div id=secondSection class=section>
    Second section content goes here...
</div>
```

This is it - now the sections are shipped with additional properties
and you can fetch them on viewport scroll and reflect the scrolling
state in an indicator:

```js
// use document.body if the whole page is scrollable
var viewport = document.getElementById('myViewport');
var firstSection = document.getElementById('firstSection');

viewport.addEventListener(
    'scroll',
    function() {
        var location = firstSection.viewportTopLocation;
        console.log(
            'The viewport is at ' + location +
            ' relatively to the first section'
        );
    },
    false
);
```


Section elements are equipped with the following properties:

- `viewportTopLocation` - vertical scrolling position of the viewport
  relatively to the section. If the section is currently visible in
  the viewport, the number is between 0 (section start) and 1 (section
  end). Value < 0 means that section is below the viewport; value > 1
  means that the section is above the area displayed in the
  viewport. The `viewportTopLocation` property designates a progress
  of the viewport scrolling through the section. Since a viewport is
  not a point, the `viewportTopLocation` property actually designates
  a position of a special point within a viewport somwhere between the
  viewport top and bottom edges. The point moves from top to bottom of
  the viewport as long as the viewport is scrolled.

- `viewportTopStart` - a number designating the current location of
  the top edge of the viewport relatively to the section. Value has
  the same meaning as for the `viewportTopLocation` property, but
  `viewportTopStart` represents the exact location of the top edge,
  and not of the viewport as a whole.

- `viewportTopEnd` - similar property reflecting the position of the
  viewport bottom edge. You will need to use these two properties if
  you wish to display a viewport position as a range (like on a
  scrollbar).

There are also the similar properties for the horizontal dimension:

- `viewportLeftLocation` - horizontal scrolling progress of a viewport
  relatively to the section;

- `viewportLeftStart` - viewport left edge position;

- `viewportLeftEnd` - veiwport right edge position;

The following properties can be used to determine where the viewport
should be scrolled programmatically in order to display the beginning
of the section (or to put the section in the center of the viewport,
in case if the section is small enough to be fully displayed within
the viewport):

- `viewportScrollTopTarget`

- `viewportScrollLeftTarget`

You will need these properties if you have a navigation component
which should scroll the viewport to the given section upon click. In
this case also have a look at the [natural
scroll](http://github.com/asvd/naturalScroll) library which enables
smooth and natural programmatical scrolling (it is enabled on the
[demo page](http://asvd.github.io/viewport)).

If a viewport is not the whole page, add the `viewport` class to the
viewport element (it should be the element which actually performs
scrolling):


```html
<div class=viewport>
  <div id=firstSection class=section>
      First section content goes here...
  </div>

  <div id=secondSection class=section>
      Second section content goes here...
  </div>
</div>
```


A viewport element also exposes the `currentSection` property which
points to the section element currently displayed in the viewport
(more precisely, the section which is the closest to the viewport):


```js
var currentSection = document.getElementById('myViewport').currentSection;
```


If you are going to use the `viewport.js` library, you are likely
about to create a navigation component. This component will be
relevant to the particular application / web-page and therefore should
provide more convenient navigation than the ordinary scrollbar. In
this case it might be reasonable to replace the scrollbar with the
[intence](http://asvd.github.io/intence) indicator: it does not
contain active elements to control the scrolling position (but you
don't need them anymore, since you have the custom navigation), and
the scrollable area is designated in much more clear and intuitive way
(comparing to an ordinary scrollbar).


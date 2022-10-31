SVG Icons

# SVG Icons CSS #
SVG icons library in CSS background image

To add icons on website, the easiest way is to use an icon font. Simple ref link in style and after html tag in the page.

There are several well known icon fonts. Most well known is Font-Awesome, but you have others like Google Material icons.

First time searching for a font icon, we are looking for the font with the biggest number of icons (2000, 3000,... more than to have more choices "in case of". Now more and more icons gives a ref file to load bigger and bigger! Could be from 128kB to 256kB or even more (384... >750!). That means, even if downloading from a CDN, time will be at best 250ms (fiber connection) but often 750ms or more than 1 second. And that's just for the icons of the website!
5000 icons are loaded. But the website is using 8 or 10, certainly less than 20 on 5000, not really efficient.

What about building library, with just icons needed. Those icons would have to be scalable as font, can have their color changed, and can be used in regular CSS class. In add library has to be easy to setup.

---

[Test it on CodePen](https://codepen.io/sosuke/pen/Pjoqqp)

![alt text](https://github.com/pierfarrugia/svgiconsCSS/blob/main/svg_icons.webp)

[Read this on HTML page](https://aonecommunication.ch/dev/creativeprog/blog.html#svgIconsCSS)

[See it in action full screen](https://aonecommunication.ch/dev/creativeprog/content/svg_icons_CSS.html)


---

Let take the “menu” icon from Google Material font.

In PNG weight is 100B for 1 icon in 1 color. PNG is bitmap, you will have to take at least 3 or 4 sizes for 1 icon (600 to 800B) and it’s not a font, meaning you can’t change color by CSS.

In SVG, weight is 202B and we will decrease its size.

Note: if you use 10 icons, total weight will be 2kB, lot less than 250kB!

## The icon
Type “material icons” in your browser to search Google Material icon, or click here:

https://fonts.google.com/icons?selected=Material+Icons

### google icon

When in Material Icon page, type “menu” in the search, select it, a panel arrive on right. On bottom of the panel, you have 2 buttons: SVG and PNG. Click on SVG, the SVG is downloaded in your computer.

Open the SVG file in your text editor:

<svg xmlns="http://www.w3.org/2000/svg" height="24px" viewBox="0 0 24 24" width="24px" fill="#000000"><path d="M0 0h24v24H0z" fill="none"/><path d="M3 18h18v-2H3v2zm0-5h18v-2H3v2zm0-7v2h18V6H3z"/></svg>

### cleaning the svg

Last “path” is the vector description. We won’t touch it.

The path just before “” has no use, delete it.

In the first part we have height, width and fill. Remove those to obtain the following:

<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24"><path d="M3 18h18v-2H3v2zm0-5h18v-2H3v2zm0-7v2h18V6H3z"/></svg> 
We didn’t change viewbox. First 2 values are the origin x and y. 2 last are viewbox size. For us those 2 last values will give the ratio.

We have deleted height, width, and fill to be able to modify those values in the CSS and not to be polluted by inline value.

* Note: the icon is now 123B, if you need 10 -> 1.2kB *

## CSS

To define the ico we'll need 2 classes:

- ico: to define base values for all ico
- ico-XXX: to define the SVG for XXX

In add we'll need a third class for the color

- ico-YYY to define the filter color YYY

We could group the first 2 in 1 to have only 2 classes in HTML where you define ico...

First we’ll make a generic ico CSS class.

```
.ico {
display: inline-block;
width: 1em;
height: 1em;
line-height: 1em;
}
```

All sizes are 1em, and very important the line-height (needed for later vertical alignement).

Display is inline-block. Whatever HTML tag you'll be going to use, div, span, it'll be inline-block to have the size defined.

When we’ll put the class ico in any HTML element it’ll take the size defined for this element. For example if it’s a H4 of 2em, ico will be 2x1 = 2 em… Same if you are putting font-size to 36px, it'll be 36px x 1em = 36px. Because em is proportional and we put 1em everywhere, it’ll scaled regarding the element it’s in: exactly what needed.

Now let define the ico-menu class.

Remember our SVG menu was:

```
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24"><path d="M3 18h18v-2H3v2zm0-5h18v-2H3v2zm0-7v2h18V6H3z"/></svg>
```

We are putting the SVG in background-image, to do that we need the SVG with a dataURI conversion.

Copy your SVG and go here

https://www.svgbackgrounds.com/tools/svg-to-css/

![alt text](https://github.com/pierfarrugia/svgiconsCSS/blob/main/data_uri.webp)


Convert your SVG to dataURI copy result and paste the dataURI to your CSS:

```
.ico-menu{
background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 24 24'%3E%3Cpath d='M3
18h18v-2H3v2zm0-5h18v-2H3v2zm0-7v2h18V6H3z'/%3E%3C/svg%3E");
}
```
If you want to define another icon, like the close icon, do same to obtain:
```
.ico-close{
background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 24 24'%3E%3Cpath d='M19
6.41L17.59 5 12 10.59 6.41 5 5 6.41 10.59 12 5 17.59 6.41 19 12 13.41 17.59 19 19 17.59 13.41 12 19 6.41z'/%3E%3C/svg%3E");
}
```

### Color

For color we'll need to use the filter trick. SVG "fill" could be used (if not already in the SVG) with inline SVG, but here we have the SVG in background.

So we'll need to define class for each icon color. I know some can consider that boring but let think about it: how many colors are you using for your icons? 2, 3, 4... ???

Let's take white: #FFFFFF

Go here:

https://codepen.io/sosuke/pen/Pjoqqp

![alt text](https://github.com/pierfarrugia/svgiconsCSS/blob/main/filter_color.webp)

Put your #FFFFFF in the target color field, click the button "Compute filters", and you have your filter to make the class:
```
.ico-white {
filter: invert(100%) sepia(100%) saturate(0%) hue-rotate(180deg) brightness(100%) contrast(107%);
}
```

## How to use

You can use it as span or div (“ico” is inline-block).

For 1 icon you add 3 classes: the generic ico, the shape like ico-menu, and the color like ico-white.
```
<span class="ico ico-menu ico-white" style="font-size: 4em"></span>
<div class="ico ico-close ico-primary-color" style="font-size: 150%"></div>
```

For the size, you can define it in the element or leave the normal DOM hierarchy working like for a font.

The very good thing here is you can align this element with text, image (..) very easily.

## Conclusion

The first time you’ll have to make 10 or 15 icons, it could seem annoying but it’s really fast to do. You can add new icon to your library when you need.

After you'll have a CSS file with all your ico shape and ico colors. You can copy/paste those needed in your project. Your 10 needed icons would weight 1.2kB!

In the demo, you have 5 icons and 4 colors as a base for your own library

Thanks for reading

* (tested on Chrome, Firefox, Edge, Safari) *

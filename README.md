# Prosopopee

More or less a small clone of exposure.co in form of a static generator. For
those of you who don't know what exposure.co is, this more or less allows you
to organise you picture in a way that more or less tell a story.

You can find example usages here:

* http://outside.browny.pink
* http://such.life
* http://media.faimaison.net/photos/galerie/

## Requirements

Installation needs Python, pip and virtualenv

    apt-get install python-pip python-virtualenv

Gallery building needs graphicsmagick library

    apt-get install graphicsmagick

## Installation

1. Create a virtualenv, and activate it

```
virtualenv ve
source ve/bin/activate
```

2. Download and install Prosopopee

```
pip install git+https://github.com/Psycojoker/prosopopee
```

## Files organisation

The files organisation is quite simple:

* in the root directory of your project you need a settings.yaml file that will contains the title and subtitle of your gallery
* for each gallery you'll need a folder that also contains a settings.yaml file that will describe how to display the content on your gallery
* and you put the pictures of the gallery inside the gallery folder

### Root settings.yaml

The root settings.yaml should contains 2 keys : one for the title of your website and one for the subtitle. It should looks like that:

```
title: My exploration of the outside world
sub_title: it's a scary place, don't go there
```

### Gallery settings.yaml

This settings.yaml will describe:

* the title, subtitle and cover picture of your gallery that will be used on the homepage
* if your gallery is public
* the date of your gallery: this will be used on the homepage since **galleries are sorted anti chronologically** on it
* the list of sections that will contains your gallery. A section will represent either one picture, a group of pictures or text. The different kind of sections will be explained in the next README section.

Here is an example:

```yaml
title: Gallery title
sub_title: Gallery sub-title
date: 2016-01-15
cover: my_cover_picture.jpg
sections:
  - type: full-picture
    image: big_picture.jpg
    text:
      title: Big picture title
      sub_title: Some text
      date: 2016-01-15
  - type: pictures-group
    images:
      -
        - image1.jpg
        - image2.jpg
        - image3.jpg
      -
        - image4.jpg
        - image5.jpg
  - type: text
    text: Some text, HTML <b>is allowed</b>.
  - type: bordered-picture
    image: another_picture.jpg
```

And here is an example or a **private** gallery (notice the <code>public</code> keyword):

```yaml
title: Gallery title
sub_title: Gallery sub-title
date: 2016-01-15
cover: my_cover_picture.jpg
public: false
sections:
    - ...
```

### Different kind of sections

A gallery is compose of a succession of sections as you can on this [wonderfully
totally uninteresting example
gallery](http://psycojoker.github.io/prosopopee/first_gallery/) the gallery is
composed of 5 sections:

* a full screen picture with text written on it
* a picture with with borders around it
* a group of 5 pictures
* and a fullscreen picture without text on it this time

In your settings.yaml, a section will **always** have a <code>type</code> key
that will describe its kind and additional data. Underneath, the
<code>type</code> key is actually the name of an HTML template and the other
data will be passed to this template.

You can find all the sections templates here: https://github.com/Psycojoker/prosopopee/tree/master/prosopopee/templates/sections

You often have an <code>image</code> key. You need to give it a path to the
actual file. By convention, those files are put inside your gallery folder but
this is not mandatory.

#### Full Screen picture with OR without text on it

This display a full screen picture as shown in the [example
gallery](http://psycojoker.github.io/prosopopee/first_gallery/) in the first
and last sections. How you should use it:

With text:

```yaml
  - type: full-picture
    image: big_picture.jpg
    text:
      title: Big picture title
      sub_title: Some text
      date: 2016-01-15
```

Without text:

```yaml
  - type: full-picture
    image: big_picture.jpg
```

#### Bordered picture

This display a centered picture that is surrounded by white (the background) as
shown in the second position of the [example
gallery](http://psycojoker.github.io/prosopopee/first_gallery/).

How to use it:

```yaml
  - type: bordered-picture
    image: another_picture.jpg
```

#### Group of pictures

This display a group of zoomable pictures on one or multiple lines as shown on
the forth position (after the text) of the [example
gallery](http://psycojoker.github.io/prosopopee/first_gallery/).

```yaml
  - type: pictures-group
    images:
      -
        - image1.jpg
        - image2.jpg
        - image3.jpg
      -
        - image4.jpg
        - image5.jpg
```

Every sublist (the first level <code>-</code> represent a line).

**Know bug**: the images are left aligned, so if you don't put enough images on
a line, you'll have white space on the right.

#### Text

This display some centered text as shown on the third position of the [example
gallery](http://psycojoker.github.io/prosopopee/first_gallery/). HTML is
allowed inside the text.

How to use it:

```yaml
  - type: text
    text: Some text, HTML <b>is allowed</b>.
```

#### Panorama

This display a very large pictures with a drag and drop posibility on it.

How to use it:

```yaml
  - type: panorama
    image: 7.jpg
```

### Example

As a recap, here is how the files of the example gallery are organised:

```
example
      ├── settings.yaml
      └── first_gallery
          ├── settings.yaml
          └── stuff.png
```

The content of <code>example/settings.yaml</code>:

```
title: "Example gallery"
```

The content of <code>example/first_gallery/settings.yaml</code>:

```
title: my first gallery
sub_title: some subtitle
date: 2015-12-08
cover: stuff.png
sections:
  - type: full-picture
    image: stuff.png
    text:
      title: Beautiful Title
      sub_title: pouet pouet
      date: 2015-12-08
  - type: bordered-picture
    image: stuff.png
  - type: text
    text: « voici plein de blabla à rajouter et <b>ceci est du gras</b> et encore plein plein plein plein de text car je veux voir comment ça va wrapper car c'est important et il faut pas que j'oublie de mettre des margins en % sinon ça va pas le faire alala là ça devrait aller »
  - type: pictures-group
    images:
      -
        - stuff.png
        - stuff.png
        - stuff.png
      -
        - stuff.png
        - stuff.png
  - type: full-picture
    image: stuff.png
```

## Build the website

**Note: You need to be in an activated virtualenv.**

In a folder containing the **root** settings.yaml file, simply do

    prosopopee

A `build` folder will be created in the current directory, containing an
index.html, static files (css & js) and pictures.

## Credit

> 16:57 &lt;meornithorynque> et tu as besoin d'un nom ?<br>
> 16:57 &lt;meornithorynque> genre n'importe quoi ?<br>
> 16:57 &lt;meornithorynque> je propose "Prosopopée"
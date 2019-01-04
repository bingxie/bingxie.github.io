---
layout: post
title: Add new font for ImageMagick on Mac OS X and Ubuntu
categories: [Tech]
tags: [imagemagick, font]
---

![](/images/Bing_707.JPG)


## Use Case

I am developing a Rails application which can add some beautiful text on the image. So I used the powerful tool `ImageMagick`(wrapped by gem [MiniMagick](https://github.com/minimagick/minimagick)).

As I said I need to make the text looks beautiful, so I have to install some fonts on both my Mac OS X and Ubuntu Server which can be used for ImageMagick. For example, this beautiful [Lato](http://www.latofonts.com/) fonts.

## on Ubuntu Server

Actually, on Ubuntu, there is already a package to intall. It's very straightforward.

### 1. Install fonts-lato

	sudo apt-get update
	sudo apt-get install fonts-lato

### 2. Find the font name to use

Find the available font list with:

	mogrify -list font | grep Lato

	#output
	Font: Lato-Medium
	  family: Lato
	  glyphs: /usr/share/fonts/truetype/lato/Lato-Medium.ttf
	Font: Lato-Medium-Italic
	  family: Lato
	  glyphs: /usr/share/fonts/truetype/lato/Lato-MediumItalic.ttf
	Font: Lato-Regular
	  family: Lato
	  glyphs: /usr/share/fonts/truetype/lato/Lato-Regular.ttf

Then, just grab a font name like 'Lato-Regular' to use in the code.

## on Mac OS X

### 1. Install Lato fonts

Download the Lato fonts from [here](http://www.latofonts.com/lato-free-fonts/#download). Unzip the download file. And copy any `.ttf` font files which you want to use to `/Library/Fonts`

### 2. Generate type.xml for ImageMagick

Make a new directory for ImageMagick local settings and cd into it

	mkdir ~/.magick
	cd ~/.magick

Grab the script to find all fonts and store them in a config file

	curl http://www.imagemagick.org/Usage/scripts/imagick_type_gen > type_gen

	find /Library/Fonts -name *.ttf -o -name *.otf | perl type_gen -f - > type.xml

Go to ImageMagick config folder

	cd /usr/local/Cellar/imagemagick/6.9.3-7/etc/ImageMagick-6

Edit system config file called `type.xml` and add line near end to tell ImageMagick to look at local file we made in earlier step

	<typemap>
	  <include file="type-ghostscript.xml" />
	  <include file="~/.magick/type.xml" />  ### THIS LINE ADDED ###
	</typemap>

### 3. Find the font name to use

Find the available font list with:

	mogrify -list font | grep Lato

	### output
	Font: LatoM
     family: Lato Medium
     glyphs: /Library/Fonts/Lato-Medium.ttf
	Font: Lato
     family: Lato
     glyphs: /Library/Fonts/Lato-Regular.ttf

Because the script generated the name is not as same as it is on Ubuntu Server. So we can manually edit the `~/.magick/type.xml` file and change the name, then in the code we use the same font name.

---
layout: post
title: "Coffeescript and QR Codes"
description: "Generate beautiful, colored QRCodes with Coffescript and HTML5 SVG"
category: 'code'
tags: ['coffeescript' , 'html5']
image : qrcode.png
---
{% include JB/setup %}

The emerging prevalence of QR codes is slowly turning the urban landscape into a hyperlinked, cyber-punk augmented reality. These blocky, alien codes connect the terrestrial landscape with the limitless possibility of the Internet.

For generating your own QR codes and turning your neighborhood into a Snowcrash-esque hyperreality; I present Blocky: A Client-side QR code library written in CoffeeScript.

Blocky began as a way to generate configurable QR codes without relying on any 3rd party service. All QR code generation is performed clientside and displayed via HTML5 . I’m currently adding support for SVG and other types of display methods should the need arise. At it’s core, Blocky uses the awesome SVG library created by Kazuhiko Arasé. [1]

**Let’s generate some funky QR codes!**

The constructor signature looks like this:

new Blocky( QR\_code\_message, id\_of\_container , config_object)

A  element is appended to the container. 

The config object takes the following values:

*   **cell_size**: the size of the QR cells. Generally just think of it as the size of the QR code. Default 4.
*   **black**: the color to use as ‘black’ in the QR code. pass as a quoted rgb value like “rgb(1,2,3)”
*   **white**: just like black, only… white.
*   **error_level**: the QR error level to use. Acceptable values: M , H , Q, L. Default H
*   **color_scheme**: there are several default color schemes avaialble. 

Try out these color_scheme values to get started:  watermelon , wedding, arctic or spicy.

Check out the project on Github: [Blocky on GitHub][2]

References:

[1] QR code lib by Kazuhiko Arasé 



[2] QR codes Wikipedia: [][4]

 [2]: https://github.com/zmcartor/Blocky
 []: http://www.d-project.com/qrcode/index.html
 [4]: http://en.wikipedia.org/wiki/QR_code 

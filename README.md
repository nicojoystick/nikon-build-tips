# Nikon Creative Build Tips
Nikon creatives are somewhat *special*  due to the client's very strong attention to detail. They usually compare creatives side by side with the approved PSD mockups and check pixel alignment by *overlaying* it on top of the HTML converted creative. They are a big fan of crisp images that scales gracefully on retina and non-retina displays and they are always on the hunt for animation *jerkyness* and *artifacting*.

Below are some helpful tips and tricks that you can implement on your first build that will lessen client feedback and will make your overall work much more easier.

## General Tips

- When converting the mockups into HTML, make sure that you're alignment and layout is as **as close as possible** with the approved PSD design.

> Technically, we really can't make it 1:1 since photoshop pixel alignment differs from the one on the browser. Just do your best to make it as close as possible ;)

- Use **SVG** whenever you can. This includes logos, typography, and other vector based graphics. SVGs scale to any screen resolution without losing quality. 

> Most of the time, the client will provide the PSD mocks including the linked vector/eps and font files that you can convert into SVG inside Adobe Illustrator. If you don't see it or if Photoshop is giving you a warning that these linked files are missing, notify your producer so that they can request it from the client.

- If SVG is not an option, use **JPG** or **PNG** instead BUT make sure you export it **@2x**  (200%) for retina devices.

- Nikon creatives usually have multiple sizes with one *master* that will be used as the basis for the other sizes. **DO NOT** duplicate the master and resize or scale its contents to create the other creative sizes as this will appear blurry/pixelated on Webkit browsers (Chrome, Safari). Make sure you use proper sizing depending on the size of the creative you're working on.

## Development Tips

### Jerky Animations *(Client: Animations are not smooth...)* 
If you're experiencing *jerky* animations or *animations are not smooth...* feedbacks , make sure you add the following properties in your GSAP tween or timeline calls.

```javascript
force3D: false, perspective: 1000, rotation: 0.01
```

Real world example:

```javascript
TweenMax.set(lenses, {force3D: false, perspective: 1000, rotation: 0.01, autoAlpha: 0, y: 10});
```

### Blurry Images *(Client: Images are not crisp/sharp...)* 
Most of the time, the client will test the creative on both retina and non-retina displays. This type of feedback usually occurs when your creative is using images that are **@1x (non-retina)** and the client views it on a retina display.

In order to fix this issue, you need to make sure all of your images are **@2x** which you can easily accomplish in Photoshop by doing the following:

> Make sure that your PSD is created at 144 or 300 dpi. You can check this by hitting `option + cmd + i` on macOS or `alt + ctrl + i` on Windows and checking the **resolution**  field. If this is not the case, contact your producer and request for a higher resolution version of the image.

#### Exporting Images to Retina
- In your Photoshop mockup, select the layer you want to export by right clicking and selecting **Export As**.

	![Image Title](https://i.imgur.com/Yla7XJe.jpg) 

- In the **Export** window, select the format you want and set the scale value to 200% (@2x) (Notice that the width and height values will be doubled.) This is why it's important that the PSD that you're working on is created at a much higher dpi.

	![Image Title](https://i.imgur.com/0dDRr8b.png) 

- Alternatively, you can also use Photoshop's built in feature for exporting retina images by setting the **Size** value on the left panel to **2x**.

	![Image Title](https://i.imgur.com/whcQCm2.png) 

#### Using Retina Images In Your Creative
- Now that you've exported your retina-ready images, the easiest way to implement it on your creative is by making your image container size @1x (non-retina size) and setting the retina-ready image as a `background-image` with `background-size` set to `contain`. This will fit your @2x image inside your @1x image container.

	ie:

	```css
	#nikon-logo {
	    position: absolute;
	    bottom: 9px;
	    right: 5px;
	    width: 42px;
	    height: 42px;
	    background-image: url(images/nikon-logo.png);
	    background-position: right bottom;
	    background-size: contain;
	    background-repeat: no-repeat;
	}
	```

### SVG to PNG/JPG Swapping (for IE10+)
Although SVGs are supported on all major browsers (including IE10+), there are some SVG features that doesn't render properly on IE10+ particularly SVGs with gradients. The SVG will render properly on IE10+ but it will not render any gradients included on the SVG file. One fix we can do is to swap to a PNG/JPG version of the said image by adding the following CSS code.

```css
/* IE10+ CSS styles go here */
@media all and (-ms-high-contrast: none), (-ms-high-contrast: active) {
    #nikon-logo-ie { display: block;  }
    #nikon-logo { display: none;  }
}

/* EDGE CSS styles go here */
@supports (-ms-ime-align: auto) {
    #nikon-logo-ie { display: block;  }
    #nikon-logo { display: none;  }
}
```

As you can see on the example above, we are basically toggling the visibility of the logo depending on which version of IE it's being viewed on.

> Please note that you need to have a dedicated style for the IE version of the graphics you want to swap out.




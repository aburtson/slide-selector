#SlideSelector

This plugin includes a module with multiple selectable slides.

This is a multi-purpose module that can be used whenever you desire a responsive layout that shows one slide at a time. When clicking a selector, the current slide content will fade out before your clicked selector's corresponding content fades in.

##Initial setup

1. _Dependencies:_ verify that the jQuery is included in your project.
2. Include **slide-selector.js** in your JS directory.
3. Include **_slide-selector.scss** or **slide-selector.css** in your stylesheets.
4. Include the HTML for the first `.ss` object inside **slide-selector.html**. If you want to use a version with images instead of labels, use the `.ss.ss--images` code instead, and refer to __.ss--images__ section at the bottom of this readme.
5. Run gulp, and your slideSelector will be up and running.

##Working with HTML

###Adding new slides

If you want to add another slide to your slideSelector, insert another `<li>` inside `.ss__selectors` to add a new label, as well as another `<div>` inside `.ss__slides` to add corresponding slide content, like so:

```html
<div class="ss">
	<ul class="ss__selectors">
		<li>...</li>
		<li>...</li>
		<li>...</li>
		<li></li> <!-- your new li -->
	</ul>
	<div class="ss__slides">
		<div>...</div>
		<div>...</div>
		<div>...</div>
		<div></div> <!-- your new div -->
	</div>
</div>
```

Make sure to insert both elements in the same spot relative to each other (ie, last, first, second, etc).

Feel free to remove slides if need be. The HTML includes three slides by default, but two works fine as well.

##Working with JS

###The slideSelector function

At the bottom of the __slide-selector.js__ file, you will find this function:

```javascript
$('.ss').slideSelector();
```

This function creates your slideSelector.

If you want to customize the display settings of your slideSelector, include any of the below options, like so:

```javascript
$('.ss').slideSelector({
    initialSlide: 2,
    speed: 200,
    breakpoint: 900,
    mobileStyle: 'toggle',
    mobileOnly: false,
    togglesOpen: true
});
```

####initialSlide

Sets which slide is active, `1` being the first. Default is `1`.

####speed

Sets the fade transition time (in milliseconds). Default is `300`.

####breakpoint

Determines the mobile breakpoint. If screen width is less than or equal to this number (in pixels), the mobile layout will be shown. Default is `768`.

####mobileStyle

Provides alternative mobile layouts.

* `'list'`: displays all slide content vertically, paired with labels.
* `'toggle'`: displays all slide content vertically, paired with labels which can be clicked to show/hide the content. Content is hidden by default.

If mobileStyle is not defined, or the above options are not selected, your slideSelector will show the desktop layout on all screen sizes.

####mobileOnly

If `true` and if `mobileStyle` is defined, the chosen mobile layout will replace the desktop layout at every screen size. Default is `false`.

####togglesOpen

If `true` and if `mobileStyle` is set to `'toggle'`, content for all slides will be visible on mobile layouts.

###Creating multiple versions

slideSelector only works on the provided HTML base structure, which has the class `.ss`. If you want to target all slideSelectors on your site with the same settings, keep the function like so:

```javascript
$('.ss').slideSelector();
```
 
Otherwise, you can add classes or ids to different instances of the HTML, and run another function, like so:
 
```javascript
$('.your-class').slideSelector();
$('.your-other-class').slideSelector();
```
 
You can keep these functions inside the __slide-selector.js__ file, or add it to another JS file, as long as it is included on your site after __slide-selector.js__.

####Unique settings for each version

You can even add different settings in each function for further customization, like so:

```javascript
$('.your-class').slideSelector({
    initialSlide: 3,
    speed: 400,
    breakpoint: 480,
    mobileStyle: 'list'
});

$('.your-other-class').slideSelector({
    speed: 200,
    mobileStyle: 'toggle',
    mobileOnly: true
});
```

__Note:__ If you do create different versions of the slideSelector, make sure to avoid including the default `$('.ss').slideSelector();` on the same HTML page as your other slideSelector versions. 

##Working with CSS


###Mobile modifiers
There are a few important modifier classes to know about when adding your own styles:

* `.ss--mobile`: added to the slideSelector when the mobile layout is visible. Check your `breakpoint` settings in your JS to know when your mobile layout will be visible.
* `.ss--mobile--list`: added to the slideSelector if `mobileStyle` is set to `'list'`.
* `.ss--mobile--toggle`: added to the slideSelector if `mobileStyle` is set to `'toggle'`.
* `.ss--mobile--default`: added to the slideSelector if `mobileStyle` is undefined. Out of the box, no styles are assigned to this class (so there will be no visible mobile layout changes), although `.ss--mobile` will still be added to the slideSelector based on your `breakpoint` setting, so feel free to take advantage of this.

###Additional elements

The JS adds additional elements to the HTML in order to provide alternative layouts for mobile. Below is an explanation of where to find these elements, and what they do.

If you inspect a slide in your browser using developer tools, you will find that the layout is different than in your original HTML. It will look something like this:

```html
<div class="ss">
	<ul class="ss__selectors">...</ul>
	<div class="ss__slides">
		<div>
			<div class="ss__label">...</div>
			<div class="ss__content">
				<div class="ss__content__inner">...</div>
			</div>
		</div>
		...
	</div>
</div>
```

Inside `.ss__slides > div`, there are the following elements:

* `.ss__label`: contains a copy of the HTML inside the corresponding `.ss__selectors > li`. This serves as the label and toggle for the slide content on mobile layouts, and is hidden on the desktop layout.
* `.ss__content`: wrapper for the slide content. This is needed to set the height of the content when hiding/showing the slide content on mobile layouts.
* `.ss__content__inner`: exists inside `.ss__content`. This contains the HTML slide content initially placed inside the parent container, `.ss_slides > div`. If `mobileStyle` is set to `'toggle'`, `.ss__content__inner` will animate opacity values to fade in/out the slide content on mobile layouts.

These elements are accessible on the page at every screen size. Feel free to add your own styles, and use the mobile modifier classes explained above for more fine tuning.

##.ss--images

`.ss.ss--images` is a version of the SlideSelector that replaces each label with an image, which is enlarged in `:hover` and `.active` states.

To get started, use the second `.ss` object in the HTML as a starter. Notice that this has the added class `ss--images`. This controls all styling for this version.

You will find `img.ss__image` inside each `.ss__selectors > li` and `.ss__slides > div`. The former is visible on desktop layout, and the latter on mobile layouts.

If you want an image to display normally inside `.ss__slides > div`, make sure it does not include the class `ss__image`.

The `.ss__title` element inside `.ss__selectors > li` is optional.

Feel free to modify the styles of `.ss--images` as you desire.



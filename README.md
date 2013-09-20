## 1. Use OOCSS﻿ and break presentation into small, semantic chunks. ##
Prefer classes dealing with aspect of presentation, eg.:

		.btn.btn--positive.btn--large

Stands for a large, green button.

## 2. Extend generic component classes when creating a new one. ##
Try not to mix attributes from different components in the DOM, eg. if you're working on a component being a modified version of a primary button, instead of typing:
 
JADE

	.my-component
			button.btn.btn--primary.my-component__button

SCSS

	.my-component__button{
		// custom css goes here
	}


#### Write: ####

JADE
		
	.my-component
	  .my-component__button


SCSS

    .my-component__button{
      @extend .btn;
      @extend .btn--primary;
      
      // your custom SCSS goes here
    }



 
Sometimes your JADE code might look too verbose. If you see to many css classes attached to one DOM element, with a rather small probability of being reused later, think your architecture through, again. And again.

## 3. Use BEM methodology to break UI into small, reusable components. ##

BEM stands for **B**lock, **E**lement, **M**odifier, which is based on the idea that almost every element in the UI can be described as a container / parent (Block) with or without children (Elements). Also, the same element might have several variations in different views. This is where Modifiers come in handy.

### Example class name: ###

	
	
	// block - default component name, use single hyphens (-) to separate words
	.block-name 
	
	// element - child of a block parent, use two underscores (__)
	.block-name__child 
	
	// a modifier - modifier names are prefixed with two hyphens (--)
	.block-name--modifier 
	
	// modifier of a child (sometimes you don't need to create modifiers for parents)
	.block-name__child--modifier 
	
	// sometimes children of blocks can be block by itself too. 
	// Although is acceptable, try to avoid that and break your components into smaller pieces.
	
	.block-name__sub__child


Let's say, you're writing a basic panel component. The SCSS might look like this:

	.panel{
	  padding: 10px;
	  border: 1px solid $gray;
	  background-color: $white;
	}
	.panel__head{
	  font-size: 18px;
	  line-height: 1.4em;
	}
	
	.panel__content{
	  font-size: 16px;
	}

And the markup:

	.panel
	  .panel__head Foo bar
	  .panel__content
	    p lorem ipsum dolor sit amet consectatur adscipcit elit...



Suddenly, it turns out that the customer needs different version of it, let's say with a bold font in the title and unicorns as a background. The easiest way, thanks to BEM would be adding single new class do JADE:

	.panel.panel--cornified //- note the .panel--cornified modifier
	  .panel__head Foo bar
	  .panel__content
	    p lorem ipsum dolor sit amet consectatur adscipcit elit...


and SCSS:


	.panel{
	  padding: 10px;
	  border: 1px solid $gray;
	  background-color: $white;
	}
	.panel__head{
	  font-size: 18px;
	  line-height: 1.4em;
	}
	
	.panel__content{
	  font-size: 16px;
	}
	
	
	// Look ma, unicorns!
	.panel--cornified{
	  background-image: image-url('foo/unicorns.gif');
	
	  .panel__head{
	    font-weight: 700;
	  }
	
	}


## 4. Think in terms of reusability, create generic components ##

Try not to create page- or view-specific SCSS. Treat every view as a component.

### Don't:  ###

	.page.page--users
		h1 Foo

SCSS

	.page--users{
		@extend .page--wide;
	}

### Do: ###

	.page.page--wide //- yay, I can have more pages like this!
		h1 Foo



## 5. Use SCSS

Use @include when copying properties and @extend when generating compound selectors, eg.

### Don't ###

	@include new-button($color: $transparent);

### Do: ###

	@include border-radius() - ok

	@extend .btn;
	@extend .btn--transparent;


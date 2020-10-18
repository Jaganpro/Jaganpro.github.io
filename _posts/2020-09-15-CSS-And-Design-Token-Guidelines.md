---
layout: post
title: CSS and Design Token Guidelines
subtitle: CSS Best practices focussed around Lightning Design System
tags: [SLDS, Salesforce, Platform]
comments: true
nav-short: true
readtime: true
---

## CSS and Design Token Guidelines

## Rule 1: Use Design Tokens whenever Applicable

* Design Tokens provide systematic way to define properties (Ex: Color, Spacing, Sizing etc.)
* We use design tokens in place of hard-coded values in order to maintain consistancy and scalability
* Design Tokens are designed to be used with **CSS Custom Properties** (Native Browser Support. Finally!!! No need for SAAS or LESS Preprocessors anymore).
* Whenever we use a custom property we have to start with double dashes **--** . Otherwise the property would fail. `--lwc-` - This is used to scope `lwc` components.

```css

//CSS Custom Property
--lwc-colorBackground : rgb(243,242,242);

//Usage
.my-style {
  background-color : var(--lwc-colorBackground);
}

```

## Rule 2: Please consider reusing CSS Classes from SLDS before creating new classes

* Please consider using Utilities from SDLS before creating a new custom CSS.
* Common SLDS Utilities includes (Not limited to)
  * Alignments
  * Borders
  * Floats
  * Margin
  * Padding
  * Position
  * Size
  * Text
  * Visibility etc.
* LINK: https://www.lightningdesignsystem.com/utilities/alignment/


## Rule 3: Please use SLDS Grid System

* Please don't make your own Layouts with Custom CSS.
* SLDS Grid System is robust, flexible and provide mobile first layout support.
* They provide standard 12 column support
* LINK : https://www.lightningdesignsystem.com/utilities/grid/

## Rule 4: Customize CSS Instead of Overriding SLDS

* Whenever we want to change something about SLDS (Example: Color, Spacing, Size), avoid one-off solutions which Override the system.
* How do we know if we are overriding the system vs Customizing?
  * If we are writing styles that Cancel out SLDS Styles, this means we are overriding the System.
* This might seem like a small innocent change, but there are lot of negative effects which comes from Overriding CSS.
  * This means that we no longer are powered by Design-Token.
  * Any update to this design token will cause regression issue as not all components use tokens.
  
## Rule 5: Be Deliberate with Design Tokens

```css

//Wrong Usage
.my-card {
  margin : var(--button-spacing);
}

//Right Usage
.my-card {
  margin : var(--lwc-spacingMedium);
}
```

* Choose the right token. Some Design token are only scopes to certain elements.
* This token `button-spacing` is not scoped to any component.
* Choose universal Design token here.

## Rule 6: CSS Specificity should be low to none

```css
.button .button {...}
```

* Please don't use Type Selectors with Classes.
* It raises your CSS Specificity and creates dependency on your markup which will cause issues in future.

## Rule 7: Please Avoid Long Nested Selectors

```css
.container .sideBar .card .header .Button {...}
```
* This is a major red flag as this means CSS has Specificity issue.

## Rule 8: Please Avoid Universal Selectors

```css
.card > * {...}
```
* The range is too broad which makes it too unpredictable.
* This is setting ourselfs with CSS conflicts in future.


## Rule 9: Please Avoid `!` `Important` Declarations in CSS

```css
.button { margin: 18px !important;}
```
* There are very very few exceptions to this. But this is also a red flag.
* Please avoid `important !` declarations in CSS

## Rule 10: CSS Separation of Concerns

```css
//WRONG USAGE
.slds-scoped-notification { padding: var(--scoped-spacing);}

//RIGHT USAGE
.my-style { padding: var(--scoped-spacing);}
```

```html
<div class="slds-scoped-notification my-style" ...>
```
* Please never write CSS like the above example
* Separation of concerns here is very important.
* We recommend to keep SLDS as clean as possible.
* If you do need to write own css, Abstract your styles to your own classes.

## Rule 11: Please Avoid contextual Selectors

* Anytime we are writing CSS that is dependent on a Parent Selector, we are creating dependencies which will possibly fail in future.
* It also increases CSS specificity.
* Better way to write this is to write Flat CSS (Ie, use singular selectors).
* This will go a long way in making our CSS more predictable and modular

## Rule 12: Please follow BEM (Block Element Modifier) Methodology when writing CSS

* Please find more information here: http://getbem.com/introduction/
* The focus here is to write CSS which supports
  * Modularity
  * Reusability
  * Structure
* SLDS follows BEM to keep CSS sane and scale nicely.

## Rule 13: Please don't use Absolute Values in CSS

```css
//WRONG USAGE
.my-component{
  font-size: 14px;
  margin-top: 48px;
  width: 200px;
}

//RIGHT USAGE
.my-component{
  font-size: 1rem;
  margin-top: 3rem;
}

//SALESFORCE RIGHT USAGE
//ON TOP OF THIS WE SHOULD BE USING DESIGN TOKENS
.my-component{
  font-size: var(--lwc-fontSize5);
  margin-top: var(--lwc-spacingLarge);
}

```
* Never use `px` values. Use `rem`.
* Width should never be part of CSS classes. This should be handled by SLDS Grid System. So we remove any width attributes inside CSS.

## Rule 14: Override CSS with New Selectors
* When Overriding CSS for certain Template elements, please use new CSS Selectors as documented here

![image.png](/.attachments/image-7a81bd87-aba5-4fd4-be64-efdab3b0b508.png)

* Please use the latest Selectors for the following elements:
  * Navigation Menu
  * Panels Container
  * Record Banner
  * Record Detail
  * Record Layout
  * Record List
  * Record Related List
  * Related Articles
  * Reputation Leaderboard
  * Embedded Service Sidebar Header
  * Trending Articles by Topic

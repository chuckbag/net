- [Overview:](#overview)
- [Confirm your Color Theme](#confirm-your-color-theme)
- [Path of the Themes](#path-of-the-themes)
- [How is the Text Defined?](#how-is-the-text-defined)
- [Find the scope def](#find-the-scope-def)
- [Find undefined scopes](#find-undefined-scopes)
  - [Overview](#overview-1)
  - [Find defined scopes](#find-defined-scopes)
  - [Confirm which have scopes defined in your theme](#confirm-which-have-scopes-defined-in-your-theme)
  - [Create your own scope with its own color](#create-your-own-scope-with-its-own-color)
- [References:](#references)

# Overview: 
We like the color coding that Code does to our text.  But sometimes we want to maybe change some of the specific texts color, while keeping the same color theme.  Or we don't want to make an entirely new color theme, we just want to make some modifications to a current one.  

In this doc we will go over how to modify your theme, and see what themes are loaded.  Show you where your themes are stored in your laptop.  Show you how the different parts of the text is defined in the editor, and how those definitions are defined in your themes, and then how to modify your theme or add to it. 

# Confirm your Color Theme

In code, press `cmd`+`[shift]`+`p` to get to the Command Pallet (or Menu > View > Command Pallet).  Then type `Preferences: Color Theme`

<img src="img/2022-03-24_22-01-32.png" alt="command pallet: color theme">

Then select the theme you want.  In this example, I'm using the standard dark theme `Dark (Visual Studio)`

<img src="img/2022-03-24_22-03-34.png" alt="browse color themes">

# Path of the Themes

For a mac, look in the path: 

```/Applications/Visual Studio Code.app/Contents/Resources/app/extensions/theme-defaults/themes```

<img src="img/2022-03-24_21-38-45.png" alt="Dupe the two default color theme files">

# How is the Text Defined?

In code, press `cmd`+`[shift]`+`p` to get to the Command Pallet (or Menu > View > Command Pallet).  Then type `Developer: Inspect Editor Tokens and Scopes`

<img src="img/2022-03-24_21-39-04.png" alt="command pallet: developer inspect editor tokens and scopes">

Then place the mouse on a part of the text that you want to inspect.  In this example, we clicked on a comment section. 

<img src="img/2022-03-24_22-10-16.png" alt="inspect comment text">

# Find the scope def

Then go to the themes folder again, and open up the `dark_plus.json` and `dark_vs.json` files, and do a search for the phrase `comment` in both the docs.  

<img src="img/2022-03-24_22-17-55.png" alt="finding the color setting">

It will show you the settings for that, where you can modify the color codes for that style of text.

Once you make a change to the style, you will need to reload MS Code for the color changes to take affect.  

# Find undefined scopes

## Overview
Yea, these exist.  There are plenty of places that are defined in the code, but do not have a color definition (css scope).  

In this example, we have two different matches for markdown links.  When the link `[]()` is defined within a bold section `**[name](link)**` the link part is colored blue, and defined the foreground color `#569cd6`. 

<img src="img/2022-03-24_22-34-26.png" alt="blue md link">

But if the link is within a numbered or unnumbered list `- [name](link)`, then the link is white with the foreground color `#d4d4d4`. 

<img src="img/2022-03-24_22-33-22.png" alt="white md link">

## Find defined scopes

So what causes the link to be blue?  We go back to the definition for the blue link, and we look at the "textmade scopes".  In this case there are a bunch, and the first match will define it's color. 

<img src="img/2022-03-24_22-34-27.png" alt="blue md link with highlighted textmate scopes numbered">

## Confirm which have scopes defined in your theme

So looking at the first scope line (1), we look back into the two theme files (`dark_vs.json` and `dark_plus.json`) and search for the phrase `markup.underline`.  This scope has no color definitions, so this is not what is making the link blue.  Also, the match is for `markup.underline`, not `markup.underline.link.markdown`.  If matches on the closest match, but there is no full match.  

<img src="img/2022-03-24_22-54-50.png" alt="no color for markup.under">

Then we look at the second scope line (2), and search for the phrase `meta.link`.  This comes up with **no** matches, so this is an example of an undefined scope.   

<img src="img/2022-03-24_22-55-14.png" alt="no match for meta.link">

then we look at the third scope line (3), and search for the phrase `markup.bold`.  This comes up with the scope that defines the foreground color `#569cd6` which was the color that the text was displayed as.  

<img src="img/2022-03-24_22-55-41.png" alt="blue text for markup.bold">

## Create your own scope with its own color

In this example, the blue link test was defined with color `#569cd6`, and was defined in the theme via the third choice scope `markup.bold`.  

I like the idea of using the `markup.underline` scope, but I dont want all underlines to be my new color, only link ones.  Thus, I create a new scope titled `markup.underline.link`, and define a color (`#73b6ee`) for it. - I just copied the `markup.underline` scope and renamed it, and added the `foreground` definition.

<img src="img/2022-03-24_22-39-30.png" alt="new scope in theme: markup.underline.link">

Then after restarting Code, when I look back at the link, I see that they are all color coded properly, with the new link color `#73b6ee`!

<img src="img/2022-03-24_22-42-15.png" alt="proof that the color change works">

# References: 
- https://www.youtube.com/watch?v=Su-cNLe0dgw

<img src="img/" alt="">
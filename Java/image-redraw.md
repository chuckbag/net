# Image Redraw


## Overview: 
The following webpage will show the two images (in red "Image1.jpg" and "Image2.jpg"), and will redraw the images every 2 seconds.  

## Code: 
```
<html>
<head>
    <title>image-redraw-1</title>
    <!-- <META HTTP-EQUIV="refresh" CONTENT="15"> -->
    <script language="JavaScript"><!--
    function refreshIt() {
        if (!document.images) return;
        document.images['img1'].src = 'Image1.jpg' + Math.random();
        document.images['img2'].src = 'Image2.jpg' + Math.random();
        setTimeout('refreshIt()',2000); // refresh every 2 secs
    }
    //--></script>
</head>

<body  onLoad=" setTimeout('refreshIt()',2000)">
    Images redraw every 2 seconds. 
    <p>
    <img src="Image1.jpg" name="img1" />
    <p>
    <img src="Image2.jpg" name="img2" />
    <p>
</body>

</html>
```

## Discussion: 
To do this on you own, all the purple code is normal everyday html stuff.  The green is the little java script.  
For each of the `Image#.jpg` you want to add to your html page, define them in the script section as well, and make sure that you have matching reference names.  You can change the amount of time between each refresh in the two `2000` numbers.   

Note that in the Java section does not have the same context rules as the html one.  So if you are polling a monitoring page (like ganglia or something) and you are using some regex in the url, it might not work in the screen redraw.  Have a look at the following html and ASCII codes.  They might need to be converted for your image to redraw properly. 

- [HTML Codes Table, Characters and symbols](http://www.ascii.cl/htmlcodes.htm): 
- [ASCII Codes For Non-Alphanumeric Printable Characters](https://users.cs.dal.ca/~jamie/CS4173/examples/XHTML/entities/ASCII.html): J. Blustein, Dec 2003

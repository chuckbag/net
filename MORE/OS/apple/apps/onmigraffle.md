# OmniGraffle


How to make two circles with a link between them: 
```
property pstrArrow : "FilledArrow"
property plstRed : {65535, 0, 0}
property plstCenter : {0, 0}

tell application id "OGfl"
 tell (make new document)
 tell front canvas
 -- DISABLE AUTOMATIC LAYOUT
 set automatic layout of its layout info to false
 
 -- GET RELATIVE UNITS
 set {rX, rY} to page size
 set rSize to rX / 10
 set rDown to rY / 3
 
 -- MAKE SHAPES WITH MAGNETS AT THEIR CENTERS
 set shapeA to make new shape with properties {name:"Circle", origin:{rDown, rDown}, size:{rSize, rSize}, magnets:{plstCenter}}
 set shapeB to make new shape with properties {name:"Circle", origin:{rDown + (rSize * 1.3), rDown}, size:{rSize, rSize}, magnets:{plstCenter}}
 
 -- CONNECT THE SHAPES WITH A LINE
 tell (make new line with properties {stroke color:plstRed, head type:pstrArrow}) to set {source, destination} to {shapeA, shapeB}
 end tell
 end tell
end tell
```

# Ref: 
- https://omni-automation.com/omnigraffle/graphics-02.html
- https://omni-automation.com/tutorial/og-macos/index.html
- http://forums.omnigroup.com/showthread.php?t=20944

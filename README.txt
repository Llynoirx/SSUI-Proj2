
KEY DEFINITIONS
- springs: stretch & compress
- struts: fixed

FILES
- DrawnObjectBase.ts: basic ops for hierarchical objects that draw output on canvas
    - pos of object: (x,y,w,h) => bounding box
    - size config: min, nat, maxWidth, maxHeght (wConfig, hConfig), visible
    - hierarchical coord system: obj put at (x,y) within parent; then has local coord, with TL = origin
    - layout: 
        1. Bottom-up: determine available size ranges
        2. Top-down: Establish final size and pos of objs
        - each config have min, nat, desired, max size of objs
    - damage mangement: when obj change, update visual img of obj 
    - draw(): draw display for obj + all its children in local coord
    - TODO: 
        1. set x pos property (declare damage when smthg changes)
        2. set y pos property 
        3. set w property
        4. set wConig property 
        5. set h property 
        6. set hConfig property
        7. set visible 
        8. applyClip(): Reduce clipping area to intersection of existing clipping area and rect 
        9. _startChildDraw(): Setup context to draw child at idx 
            - (save curr state of context obj, apply translation transformation to move to child coord, reduce clipping region to remain in child bounding box)
        10. damageArea(): Declare display in local damaged (no longer correct); have it redrawn at next opportunity
        11. _damageFromChild(): Get damage report from child obj: local coords, corresponding damage up tree via parent

- TopObject.ts: Root/top obj of drawing obj tree
    - drawing, damage mangement, layout, redraw processes happen here
    - TODO: 
        1. _drawSelfOnly(): Clear canvas behind children drawn 
        2. layoutAndDrawAll(): Invoke layout + redraw of tree
        3. damageArea(): override damage routine

- TextObject.ts: Display single text string on one line
    - TODO: 
        1. set text 
        2. set font 
        3. set padding 
        4. _recalcSize(): recalculate obj size based on text size
        5. _drawSelfOnly(): draw obj (left-to-right Latin alphabet)

- FilledObject.ts: Fill bounding box w/ color
    - TODO: 
        1. override set w to enforce fixed size
        2. override set h to enforce fixed size
        3. _drawSelfOnly(): Draw filled rectangle content for obj 

- Strut.ts: Put btwn content objs of row/col by providing fixed spacing; no drawing output
    - TODO: 
    1. override set w to enforce fixed size
    2. override set h to enforce fixed size

- Column.ts: Column layout to work w/ springs and struts
    - height inputted (fixed)
    - width automatically set (elastic)
    - _doChildSizing(): ensure child size config up to date
    - _adjustChildren(): Adjust child height to do vertical springs + struts
    - TODO: 
        1. _doLocalSizing(): set height and width config based on children
        2. _measureChildren(): Measure children to prep for adjusting sizes 
            - (natSum of non-spring children, availCompr: total available compression across non-spring, numSprings of child)
        3. _expandChildSprings(): expand child springs by adding excess space to total space across springs
        4. _compressChildren(): compress/contract children to make up for shortfall
        5. _completeLocalLayout(): Set final size and position of immediate children of local top down pass

- Row.ts: Row layout to work w/ springs and struts
    - width inputted 
    - height automatically set to fixed size
    - TODO:
        1. _doLocalSizing (): set height and width config based on children 
        2. _measureChildren(): Measure children to prep for adjusting sizes 
        3. _expandChildSprings(): expand child springs by adding excess space to total space across springs
        4. _compressChildren(): compress/contract children to make up for shortfall
        5. _completeLocalLayout(): Set final size and position of immediate children of local top down pass

- DrawableImage.ts: Draw canvas + arrange loading notif
    - DrawableImage{url, notifyFun}
    - addNotify(): Once img loaded, add notif ftn 
    - _startLoad(): Assuming have url and added callback ftns for load notifs, start loading img obj encapsulates
    - reload(): reload context 

- SizeConfig.ts: Size config obj (min, nat, max)

- Util.ts: object, text measurement literals, limited value tring types 

- Err.ts: handles exceptions through the following:
    - silent: ignore error
    - message: print exception message to console log
    - full_mesage: print exc message + stack trace to console log
    - throw: rethrow exception 

- test_cases.ts: Test cases/demonstration of code for proj

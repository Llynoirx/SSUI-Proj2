
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
        - [x] set x pos property (declare damage when smthg changes)
        - [x] set y pos property
        - [x] set w property
        - [x] set wConig property 
        - [x] set h property 
        - [x] set hConfig property
        - [x] set visible 
        - [x] applyClip(): Reduce clipping area to intersection of existing clipping area and rect 
        - [x] _startChildDraw(): Setup context to draw child at idx 
            - (save curr state of context obj, apply translation transformation to move to child coord, reduce clipping region to remain in child bounding box)
        - [x] damageArea(): Declare display in local damaged (no longer correct); have it redrawn at next opportunity
        - [x] _damageFromChild(): Get damage report from child obj: local coords, corresponding damage up tree via parent

- IconObject.ts: Obj that draws image (icon)
    - TODO:
        - [x] set _resizesImage: if true, img -> obj size, else set obj size -> img size
        - [x] _resize: if size determined by img, obj -> img size, else nothing
        - [x] _drawSelfOnly: draw object 

- TopObject.ts: Root/top obj of drawing obj tree
    - drawing, damage mangement, layout, redraw processes happen here
    - TODO: 
        - [x] _drawSelfOnly(): Clear canvas behind children drawn 
        - [x] layoutAndDrawAll(): Invoke layout + redraw of tree
        - [x] damageArea(): override damage routine

- TextObject.ts: Display single text string on one line
    - TODO: 
        [] set text 
        [] set font 
        [] set padding 
        [] _recalcSize(): recalculate obj size based on text size
        [] _drawSelfOnly(): draw obj (left-to-right Latin alphabet)

- FilledObject.ts: Fill bounding box w/ color
    - TODO: 
        [] override set w to enforce fixed size
        [] override set h to enforce fixed size
        [] _drawSelfOnly(): Draw filled rectangle content for obj 

- Strut.ts: Put btwn content objs of row/col by providing fixed spacing; no drawing output
    - TODO: 
        [] override set w to enforce fixed size
        [] override set h to enforce fixed size

- Column.ts: Column layout to work w/ springs and struts
    - height inputted (fixed)
    - width automatically set (elastic)
    - _doChildSizing(): ensure child size config up to date
    - _adjustChildren(): Adjust child height to do vertical springs + struts
    - TODO: 
        [] _doLocalSizing(): set height and width config based on children
        [] _measureChildren(): Measure children to prep for adjusting sizes 
            - (natSum of non-spring children, availCompr: total available compression across non-spring, numSprings of child)
        [] _expandChildSprings(): expand child springs by adding excess space to total space across springs
        [] _compressChildren(): compress/contract children to make up for shortfall
        [] _completeLocalLayout(): Set final size and position of immediate children of local top down pass

- Row.ts: Row layout to work w/ springs and struts
    - width inputted 
    - height automatically set to fixed size
    - TODO:
        [] _doLocalSizing (): set height and width config based on children 
        [] _measureChildren(): Measure children to prep for adjusting sizes 
        [] _expandChildSprings(): expand child springs by adding excess space to total space across springs
        [] _compressChildren(): compress/contract children to make up for shortfall
        [] _completeLocalLayout(): Set final size and position of immediate children of local top down pass

- DrawableImage.ts: Draw canvas + arrange loading notif
    - DrawableImage{url, notifyFun}
    - addNotify(): Once img loaded, add notif ftn 
    - _startLoad(): Assuming have url and added callback ftns for load notifs, start loading img obj encapsulates
    - reload(): reload context 

- SizeConfig.ts: Size config obj (min, nat, max)

- Group.ts: Group child objs together

- Util.ts: object, text measurement literals, limited value tring types 

- Err.ts: handles exceptions through the following:
    - silent: ignore error
    - message: print exception message to console log
    - full_mesage: print exc message + stack trace to console log
    - throw: rethrow exception 

- test_cases.ts: Test cases/demonstration of code for proj

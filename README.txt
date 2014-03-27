3d-barCss
=========

3d bar In Css 
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html xmlns="http://www.w3.org/1999/xhtml">
<head>
    <title>Drag and Drop Test</title>
    <style type="text/css">
        .body
        {
            top: 0px;
            left: 0px;
            width: 1024px;
            height: 768px;
            color: green;
        }
        #abc
        {
            position: absolute;
            width: 300px;
            height: 300px;
            background-color: rgb(0,0,0);
        }
        #tot
        {
            position: absolute;
            top: 100px;
            right: 100px;
        }
        
        #drop
        {
            position: absolute;
            top: 400px;
            left: 400px;
            width: 500px;
            height: 500px;
            border: 2px solid black;
        }
    </style>
</head>
<body>
    <div class='body'>
        <div id="drop">
            Hey Drop here!!!
        </div>
        <div id='abc'>
            <img src="mask.png" />
        </div>
        <div id='tot'>
        </div>
    </div>
</body>
<script src="scripts/jquery.js" type="text/javascript"></script>
<script type="text/javascript">
    $.fn.dragNdrop = function (o) {
        var that = this;
        var flag = false;
        var posX = 0, posY = 0;
        var top = 0, left = 0;

        var dropElements = [];

        for (var elementId in o.dropElementIds) {
            dropElements.push({
                element: $('#' + elementId),
                top: this.element.position().top,
                left: this.element.position().left,
                right: this.element.position().left + this.element.width(),
                bottom: this.element.position().top + this.element.height()
            });
        }

        //alert(dropElement.element.width());

        that.css({
            '-webkit-touch-callout': 'none',
            '-webkit-user-select': 'none',
            '-khtml-user-select': 'none',
            '-moz-user-select': 'none',
            '-ms-user-select': 'none',
            'user-select': 'none'
        });

        this.checkForDrop = function (x, y) {
            console.log('mouse move triggered' + dropElements[0].top);
            for (var dropElement in dropElements) {
                if (x > dropElement.left && x < dropElement.right && y > dropElement.top && y < dropElement.bottom) {
                    dropElement.element.css({
                        'border-color': 'yellow'
                    });
                }
                else {
                    dropElement.element.css({
                        'border-color': 'black'
                    });
                }
            }



        }
        this.onDrop = function () {

        }
        this.on('mousedown touchstart', function (event) {
            event.preventDefault();
            flag = true;
            console.log('mouse down triggered');
            //alert(event.originalEvent.touches);
            posX = event.pageX;
            posY = event.pageY;
            top = that.position().top;
            left = that.position().left;
        });

        this.on('mousemove touchmove', function (event) {
            event.preventDefault();
            var ev = event.originalEvent.touches ? event.originalEvent.touches[0] : event;

            if (flag) {
                document.getElementById('tot').innerHTML = 'top' + Math.round(top + (ev.pageY - posY));
                that.css({
                    'position': 'absolute',
                    'top': Math.round(top + (ev.pageY - posY)) + 'px',
                    'left': Math.round(left + (ev.pageX - posX)) + 'px'
                });

                that.checkForDrop(ev.pageX, ev.pageY);
            }

        });

        $(document).on('mouseup touchend', function (event) {
            event.preventDefault();
            flag = false;
            console.log('mouse up triggered' + event.target);
        });

    };

    $("#abc").dragNdrop({ dropElementId: 'drop' });


</script>
</html>

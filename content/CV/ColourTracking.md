+++
Categories = ["CV", "Computer Vision"]
Description = "Tracking a colour in an image using tracking.js"
Tags = ["Computer Vision", "CV", "OpenCV"]
date = "2015-06-13T00:35:28+05:30"
menu = "CV"
title = "Colour Tracking"
weight = 2
+++

A small example to track single colour in an Image.

<script src="/scripts/cv/tracking-min.js"></script>
<video id="myVideo" width="200" height="150" preload autoplay loop muted></video><br>
<canvas id="trackingOutput" class = "outputchannel" width="200" height="150" style = "border:1px solid #000000"></canvas>

<script>
    outputCanvas = document.getElementById('trackingOutput');
    outputContext = outputCanvas.getContext('2d');
    outputImageData = outputContext.createImageData(outputCanvas.width, outputCanvas.height);
    
    trackColor = [120, 120, 120];
    inRange = function(color1, color2) {
        delta = 10;
        diff = color1 > color2 ? color1 - color2 : color2 - color1;
        return diff <= delta;
    }
    
    tracking.MyTracker = function() {
        tracking.MyTracker.base(this, 'constructor');
    }
    tracking.inherits(tracking.MyTracker, tracking.Tracker);

    tracking.MyTracker.prototype.track = function(pixels, width, height) {
        // Your tracking logic here
        trackedImageData = new Uint8Array(pixels.length);

        height = outputCanvas.height
        width = outputCanvas.width
        for (i = 0; i < width; i++) {
            for (j = 0; j < height; j++) {
                index = (i + j * width) * 4;
                if (inRange(pixels[index + 0], trackColor[0]) && 
                        inRange(pixels[index + 1], trackColor[1]) && 
                            inRange(pixels[index + 2], trackColor[2])) {
                    trackedImageData[index + 0] = 255;
                    trackedImageData[index + 1] = 255;
                    trackedImageData[index + 2] = 255;
                }
                trackedImageData[index + 3] = pixels[index + 3];
            }
        }

        this.emit('track', {
            output: trackedImageData,
        });
    }

    var myTracker = new tracking.MyTracker()
    myTracker.on('track', function(event) {
        height = outputCanvas.height
        width = outputCanvas.width
        for (i = 0; i < width; i++){
            for (j = 0; j < height; j++) {
                index = (i + j * width) * 4;
                outputImageData.data[index + 0] = event.output[index + 0];
                outputImageData.data[index + 1] = event.output[index + 1];
                outputImageData.data[index + 2] = event.output[index + 2];
                outputImageData.data[index + 3] = event.output[index + 3];
            }
        }
        outputContext.putImageData(outputImageData, 0, 0);

    })
    window.onload = function() {
        // note here that 'camera' is set to true, I believe this tells tracking.js to use
        // the webcam.
        tracking.track('#myVideo', myTracker, {
            camera: true
        });
  }
</script>
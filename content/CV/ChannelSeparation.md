+++
Categories = ["CV", "Computer Vision"]
Description = "Extract 3 channels from input RGBA image"
Tags = ["Computer Vision", "CV", "OpenCV"]
date = "2015-06-24T20:36:57+05:30"
menu = "CV"
title = "Extract RGB Channels"
weight = 3
+++

<script src="/scripts/cv/tracking-min.js"></script>
<video id="myVideo" width="200" height="150" preload autoplay loop muted></video><br>
<canvas id="channelR" class = "outputchannel" width="200" height="150"></canvas>
<canvas id="channelG" class = "outputchannel" width="200" height="150"></canvas>
<canvas id="channelB" class = "outputchannel" width="200" height="150"></canvas>

<script>
    canvasR = document.getElementById('channelR');
    canvasG = document.getElementById('channelG');
    canvasB = document.getElementById('channelB');

    contextR = canvasR.getContext('2d');
    contextG = canvasG.getContext('2d');
    contextB = canvasB.getContext('2d');

    imageDataR = contextR.createImageData(canvasR.width, canvasR.height);
    imageDataG = contextG.createImageData(canvasG.width, canvasG.height);
    imageDataB = contextB.createImageData(canvasB.width, canvasB.height);

    tracking.MyTracker = function() {
        tracking.MyTracker.base(this, 'constructor');
    }
    tracking.inherits(tracking.MyTracker, tracking.Tracker);

    tracking.MyTracker.prototype.track = function(pixels, width, height) {
        // Your tracking logic here
        pixelsRed = new Uint8Array(pixels.length);
        pixelsGreen = new Uint8Array(pixels.length);
        pixelsBlue = new Uint8Array(pixels.length);

        height = canvasR.height
        width = canvasR.width
        for (i = 0; i < width; i++) {
            for (j = 0; j < height; j++) {
                index = (i + j * width) * 4;
                pixelsRed[index + 0] = pixels[index + 0];
                pixelsGreen[index + 1] = pixels[index + 1];
                pixelsBlue[index + 2] = pixels[index + 2];
            }
        }

        this.emit('track', {
            channel: {
            	red: pixelsRed,
            	green: pixelsGreen,
            	blue: pixelsBlue
            }
        });
    }

    var myTracker = new tracking.MyTracker()
    myTracker.on('track', function(event) {
        height = canvasR.height
        width = canvasR.width
        for (i = 0; i < width; i++){
            for (j = 0; j < height; j++) {
                index = (i + j * width) * 4;
                imageDataR.data[index + 0] = event.channel.red[index + 0];
                imageDataG.data[index + 1] = event.channel.green[index + 1];
                imageDataB.data[index + 2] = event.channel.blue[index + 2];

                imageDataR.data[index + 3] = 255;
                imageDataG.data[index + 3] = 255;
                imageDataB.data[index + 3] = 255;

            }
        }
        contextR.putImageData(imageDataR, 0, 0);
        contextG.putImageData(imageDataG, 0, 0);
        contextB.putImageData(imageDataB, 0, 0);

    })
    window.onload = function() {
        // note here that 'camera' is set to true, I believe this tells tracking.js to use
        // the webcam.
        tracking.track('#myVideo', myTracker, {
            camera: true
        });
  }
</script>

<style type="text/css">
	.outputchannel {
		display: inline-block;
	}
</style>
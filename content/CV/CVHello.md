+++
Categories = ["CV", "Computer Vision"]
Description = "A small example on how to access your web cam via JS using Tracking.js"
Tags = ["Computer Vision", "CV", "OpenCV"]
date = "2015-06-07T22:21:42+05:30"
menu = "CV"
title = "CV Hello World"
weight = 1
+++

Computer Vision Hello World.
My first post on how to access your webcam from JS using [tracking.js](http://trackingjs.com/)

Tracking.js is a Java script based Open Source Computer Vision library
I have just started using it. So I don't know much about its pros or cons, but it sure looks good enough to try once.

Here is a small example which is a little beyond the scope of a Hello World. You can find the [hello world examples here]()

Here, I explain the basics of how to track something (color, object etc.) based on your own logic implemented in your own custom trackers implemented over the framework designed in tracking.js


<script src="/scripts/cv/tracking-min.js"></script>
<video id="myVideo" width="400" height="300" preload autoplay loop muted></video>
<script>
    tracking.MyTracker = function() {
        tracking.MyTracker.base(this, 'constructor');
    }
    tracking.inherits(tracking.MyTracker, tracking.Tracker);

    tracking.MyTracker.prototype.track = function(pixels, width, height) {
        // Do the processing as required
        console.log(pixels.length);
        
        this.emit('track', {
            data: 'Some Data'
        });
    }

    var myTracker = new tracking.MyTracker()
    myTracker.on('track', function(event) {
        // What to do on obtaining track
    })

    window.onload = function() {
        tracking.track('#myVideo', myTracker, {
            camera: true
        });
  }
</script> 

{{< highlight html >}}
<!-- Import tracking.js library from its appropriate path -->
<script src="/scripts/cv/tracking-min.js"></script>
<!-- Display the captured frames in the specified height/width -->
<video id="myVideo" width="400" height="300" 
            preload autoplay loop muted></video>

<script>
    // Implementing the custom tracker known as MyTracker
    tracking.MyTracker = function() {
        tracking.MyTracker.base(this, 'constructor');
    }
    tracking.inherits(tracking.MyTracker, tracking.Tracker);

    // Override the track function of the tracker and 
    // implement your own logic inside here.
    tracking.MyTracker.prototype.track = 
        function(pixels, width, height) {
        // Your tracking logic here

        // Emit the generated result as a JSON object
        this.emit('output', {
            data: "Some result"
        });
    }

    // Initialise a tracker object
    var myTracker = new tracking.MyTracker();

    // Specify what is to be done on the signal
    myTracker.on('output', function(event) {
        console.log(event)
    })

    // Start tracking using the custom tracker object
    window.onload = function() {
        tracking.track('#myVideo', myTracker, {
            camera: true
        });
    } 
</script>
{{< /highlight >}}

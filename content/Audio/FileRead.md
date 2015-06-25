+++
Categories = ["Audio Processing"]
Description = "How to read an audio file using Web Audio API in HTML"
Tags = ["WebAudioAPI"]
date = "2015-06-17T20:02:17+05:30"
menu = "audio"
title = "Simple Audio Player"

+++

<div class="play icons">
	<img src = '/myIcons/play.png'>
</div>

<div class="stop icons">
	<img src = '/myIcons/pause.png'>
</div>

<style type="text/css">
	.icons {
		display: inline-block;
		height: 100px;
		width: 100px;
	}
</style>

<script type="text/javascript">
    var context, soundSource, soundBuffer, url = 'http://jatin3893.github.io/myAudio/alarm.wav';
    
    function init() {
        if (typeof AudioContext !== "undefined") {
            context = new AudioContext();
        } else if (typeof webkitAudioContext !== "undefined") {
            context = new webkitAudioContext();
        } else {
            throw new Error('Audio Context not supported. :(');
        }
    }
    
    function startSound() {
        // Note: this loads asynchronously
        var request = new XMLHttpRequest();
        request.open("GET", url, true);
        request.responseType = "arraybuffer";

        // Our asynchronous callback
        request.onload = function() {
            var audioData = request.response;

            audioGraph(audioData);
        };
        request.send();
    }

    // Finally: tell the source when to start
    function playSound() {
        // play the source now
        soundSource.start(context.currentTime);
    }

    function stopSound() {
        // stop the source now
        soundSource.stop(context.currentTime);
    }
    
    // Events for the play/stop bottons
    document.querySelector('.play').addEventListener('click', startSound);
    document.querySelector('.stop').addEventListener('click', stopSound);
    
    // This is the code we are interested in:
    function audioGraph(audioData) {
        soundSource = context.createBufferSource();
        context.decodeAudioData(audioData, function(soundBuffer){
            soundSource.buffer = soundBuffer;
    
            volumeNode = context.createGain();
    
            //Set the volume
            volumeNode.gain.value = 2.0;
    
            // Wiring
            soundSource.connect(volumeNode);
            volumeNode.connect(context.destination);
    
            // Finally
            playSound(soundSource);
        });
    }

    init();
</script>
<script>
    import { onMount } from 'svelte';
    import Tesseract from 'tesseract.js';

    let videoStream = null;
    let video;
    let canvas;

    let scannedText = 'please scan something';

	onMount( () => {
        
	});

    function setDPI( dpi) {
        // Set up CSS size.
        canvas.style.width = canvas.style.width || canvas.width + 'px';
        canvas.style.height = canvas.style.height || canvas.height + 'px';

        // Resize canvas and scale future draws.
        var scaleFactor = dpi / 96;
        canvas.width = Math.ceil(canvas.width * scaleFactor);
        canvas.height = Math.ceil(canvas.height * scaleFactor);
        var ctx = canvas.getContext('2d');
        ctx.scale(scaleFactor, scaleFactor);
    }

    async function startCamera() {
        videoStream = await navigator.mediaDevices.getUserMedia({ video: true, audio: false});
        video.srcObject = videoStream;
    }

    function stopCamera() {
        videoStream.getTracks().forEach(track => track.stop());
    }

    function takePicture() {
        // this should go in onmount or something of that nature
        const ctx = canvas.getContext('2d');
        ctx.drawImage(video, 0, 0, canvas.width, canvas.height);

        let image_data_url = canvas.toDataURL('image/jpeg');
        scannedText = 'Scanning...';
        //Convert to text
        Tesseract.recognize(
            image_data_url,
            'eng',
            { logger: m => console.log(m) }
        ).then(({ data: { text } }) => {
            scannedText = text;
            console.log(text);
        });
    }
</script>

<section>
    <video bind:this="{video}" width="320" height="240" autoplay ><track kind="captions"></video>
    <button class="btn btn-primary" on:click="{startCamera}">Start Camera</button>
    <button class="btn btn-primary" on:click="{stopCamera}">Stop Camera</button>
    <button class="btn btn-primary" on:click="{takePicture}">Take Picture</button>
    <canvas bind:this="{canvas}" width="320" height="240"></canvas>
    <input type="text" value="{scannedText}"/>

</section>
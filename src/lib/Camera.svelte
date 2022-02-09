<script>
    let videoStream = null;
    let video;
    let canvas;


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
    }
</script>

<section>
    <video bind:this="{video}" width="320" height="240" autoplay ><track kind="captions"></video>
    <button class="btn btn-primary" on:click="{startCamera}">Start Camera</button>
    <button class="btn btn-primary" on:click="{stopCamera}">Stop Camera</button>
    <button class="btn btn-primary" on:click="{takePicture}">Take Picture</button>
    <canvas bind:this="{canvas}" width="320" height="240"></canvas>
</section>
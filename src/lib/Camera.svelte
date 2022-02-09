<script>
    let videoStream = null;
    let video;

    async function startCamera() {
        videoStream = await navigator.mediaDevices.getUserMedia({ video: true, audio: false});
        video.srcObject = videoStream;
    }

    function stopCamera() {
        videoStream.getTracks().forEach(track => track.stop());
    }
</script>

<section>
    <video bind:this="{video}" width="320" height="240" autoplay ><track kind="captions"></video>
    <button class="btn btn-primary" on:click="{startCamera}">Start Camera</button>
    <button class="btn btn-primary" on:click="{stopCamera}">Stop Camera</button>
</section>
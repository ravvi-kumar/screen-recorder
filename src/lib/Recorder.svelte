<script lang="ts">
  let mediaStream: MediaStream | undefined = undefined;
  let mediaRecorder: MediaRecorder | undefined = undefined;
  async function startRecording() {
    mediaStream = await navigator.mediaDevices.getDisplayMedia();
    mediaRecorder = new MediaRecorder(mediaStream);
    mediaRecorder.start();
  }
  async function stopRecording() {
    const videoTracks = mediaStream?.getVideoTracks();
    videoTracks?.[0]?.stop();
  }

  $: mediaRecorder?.addEventListener("dataavailable", (event) => {
    const fileName = prompt(
      "Enter a name for your download file",
      "capture.wem"
    );

    if (fileName) {
      const link = document.createElement("a");
      link.href = URL.createObjectURL(event.data);
      link.download = fileName;
      link.click();
    }
  });
</script>

<button on:click={startRecording}> start Recording </button>

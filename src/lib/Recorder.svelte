<script lang="ts">
  let isRecording: Boolean = false;
  let mediaStream: MediaStream | undefined = undefined;
  let mediaRecorder: MediaRecorder | undefined = undefined;
  async function startRecording() {
    isRecording = true;
    mediaStream = await navigator.mediaDevices.getDisplayMedia({
     audio: true
    });
    mediaRecorder = new MediaRecorder(mediaStream);
    mediaRecorder.start();
  }
  async function stopRecording() {
    isRecording = false;
    const videoTracks = mediaStream?.getVideoTracks();
    videoTracks?.[0]?.stop();
  }

  $: mediaRecorder?.addEventListener("dataavailable", (event) => {
    const fileName = prompt("Enter a name for your download file", "capture");

    if (fileName) {
      const link = document.createElement("a");
      link.href = URL.createObjectURL(event.data);
      link.download = fileName + ".webm";
      link.click();
    }
  });
</script>

{#if isRecording === false}
  <button on:click={startRecording}> start Recording </button>
{:else}
  <button on:click={stopRecording}> stop Recording </button>
{/if}

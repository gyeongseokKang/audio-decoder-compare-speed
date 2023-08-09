<script lang="ts">
  let audioBuffer: AudioBuffer | null = null; // 로드된 AudioBuffer를 저장할 변수
  let audioContext: AudioContext | null = null; // AudioContext를 저장할 변수
  let isPlaying = false; // 오디오 재생 상태를 저장할 변수

  async function handleFileChange(event: Event) {
    const input = event.target as HTMLInputElement;
    const file = input.files && input.files[0];

    if (file) {
      const arrayBuffer = await file.arrayBuffer();
      audioContext = new AudioContext();

      audioContext.decodeAudioData(arrayBuffer, (decodedBuffer) => {
        audioBuffer = decodedBuffer;
      });
    }
  }

  function playAudio() {
    if (audioBuffer && audioContext) {
      const source = audioContext.createBufferSource();
      source.buffer = audioBuffer;
      source.connect(audioContext.destination);
      source.start(0);
      isPlaying = true;
    }
  }

  function stopAudio() {
    if (audioContext) {
      audioContext.close();
      audioContext = null;
      isPlaying = false;
    }
  }
</script>

<main>
  <h1>로컬 오디오 플레이어</h1>
  <input type="file" accept="audio/*" on:change={handleFileChange} />
  {#if audioBuffer}
    <button on:click={playAudio}>재생</button>
    <button on:click={stopAudio}>정지</button>
  {/if}
</main>

<style>
  main {
    text-align: center;
    padding: 20px;
  }

  h1 {
    margin-bottom: 10px;
  }

  input {
    margin-bottom: 10px;
  }
</style>

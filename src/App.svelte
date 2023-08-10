<script lang="ts">
  import { getFileAudioBuffer } from "@soundcut/decode-audio-data-fast";
  import decodeAudio from "audio-decode";
  // import * as AudioFileDecoder from "audio-file-decoder";
  // import DecodeAudioWasm from "audio-file-decoder/decode-audio.wasm";

  function decodeFileAudioData(file) {
    const audioCtx = new window.AudioContext();
    console.log(navigator.hardwareConcurrency);
    return getFileAudioBuffer(file, audioCtx);
  }

  function formatBytes(bytes) {
    const kb = bytes / 1024;
    if (kb < 1024) {
      return kb.toFixed(2) + " KB";
    } else if (kb >= 1024 && kb < 1024 * 1024) {
      return (kb / 1024).toFixed(2) + " MB";
    } else {
      return (kb / (1024 * 1024)).toFixed(2) + " GB";
    }
  }

  let audioBuffer: AudioBuffer | null = null; // 로드된 AudioBuffer를 저장할 변수
  let audioContext: AudioContext | null = null; // AudioContext를 저장할 변수
  let isPlaying = false; // 오디오 재생 상태를 저장할 변수
  let tableItem = [];
  let isDecoding = false;

  async function handleFileChange(event: Event) {
    const input = event.target as HTMLInputElement;
    const file = input.files && input.files[0];

    let item = {
      name: file.name,
      size: file.size,
      type: file.type,
      default: undefined,
      fast: undefined,
      fast2: undefined,
    };
    if (file) {
      isDecoding = true;
      let audioBuffer: AudioBuffer | null = null; // 로드된 AudioBuffer를 저장할 변수
      const arrayBuffer = await file.arrayBuffer();
      audioContext = new AudioContext();
      const prev = new Date().getTime();

      await audioContext.decodeAudioData(arrayBuffer, (decodedBuffer) => {
        audioBuffer = decodedBuffer;
        item.default = new Date().getTime() - prev;
      });

      try {
        const prev2 = new Date().getTime();
        await decodeFileAudioData(file);
        item.fast = new Date().getTime() - prev2;
      } catch (e) {
        console.log(e);
        item.fast = "error";
      }

      try {
        const prev3 = new Date().getTime();
        await decodeAudio(await file.arrayBuffer());
        item.fast2 = new Date().getTime() - prev3;
      } catch (e) {
        console.log(e);
        item.fast2 = "error";
      }

      // const prev4 = new Date().getTime();
      // AudioFileDecoder.getAudioDecoder(DecodeAudioWasm, file).then((decoder) => {
      //   // samples are returned as a Float32Array
      //   let samples;

      //   // decode entire audio file
      //   samples = decoder.decodeAudioData();

      //   console.log(samples);

      //   // ALWAYS dispose once finished to free resources
      //   decoder.dispose();
      // });
      tableItem = [...tableItem, item];

      isDecoding = false;
      console.log(tableItem);
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
  <h1>Audio Decode Compare</h1>
  <input type="file" accept="audio/*" on:change={handleFileChange} />
  {#if audioBuffer}
    <button on:click={playAudio}>재생</button>
    <button on:click={stopAudio}>정지</button>
  {/if}

  {#if isDecoding}
    <p>Decoding...</p>
  {/if}
  <table>
    <thead>
      <tr>
        <th>Name</th>
        <th>Size</th>
        <th>Type</th>
        <th
          ><a
            href="https://developer.mozilla.org/en-US/docs/Web/API/BaseAudioContext/decodeAudioData"
            target="_blank"
          >
            Default
          </a></th
        >
        <th
          ><a
            href="https://www.npmjs.com/package/@soundcut/decode-audio-data-fast"
            target="_blank"
          >
            Decode-audio-data-fast
          </a></th
        >
        <th
          ><a href="https://www.npmjs.com/package/audio-decode" target="_blank">
            audio-decode
          </a></th
        >
      </tr>
    </thead>
    <tbody>
      {#each tableItem as file}
        <tr>
          <td>{file.name}</td>
          <td>{formatBytes(file.size)}</td>
          <td>{file.type} </td>
          <td>{file.default} ms</td>
          <td>{file.fast} ms</td>
          <td>{file.fast2} ms</td>
        </tr>
      {/each}
    </tbody>
  </table>
</main>

<style>
  main {
    text-align: center;
    padding: 20px;
  }

  a {
    text-decoration: underline;
  }

  h1 {
    margin-bottom: 10px;
  }

  input {
    margin-bottom: 10px;
  }

  table {
    width: 100%;
    border-collapse: collapse;
    margin-top: 10px;
  }

  th,
  td {
    border: 1px solid #ddd;
    padding: 8px;
  }

  th {
    background-color: #f2f2f2;
  }

  tr:nth-child(even) {
    background-color: #f2f2f2;
  }
</style>

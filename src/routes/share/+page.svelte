<script lang="ts">
  import CodeMirror from "svelte-codemirror-editor";
  import { javascript } from "@codemirror/lang-javascript";
  import { oneDark } from "@codemirror/theme-one-dark";
  import { execute } from "../../lib/execute";
  import { browser } from "$app/environment";
  import { MetaTags } from "svelte-meta-tags";
  let value = "";
  if (browser) {
    const urlParams = new URLSearchParams(window.location.search);
    // get the id, ex: /share?id=123
    const id = urlParams.get("id");
    console.log(id);
    if (id) {
      fetch(`https://ad-c-9c338a775c74.herokuapp.com/code/${id}`)
        .then((res) => res.json())
        .then((data) => {
          value = data;
          console.log(data);
        });
    }
  }
</script>

<MetaTags title="Share Code Snippets!" />
<svelte:head>
  <style>
    @import url("https://fonts.cdnfonts.com/css/cascadia-code");
  </style>
</svelte:head>

<div class="center">
  <h1>Deno Online Compiler</h1>
  <h2>Note: You only have `-allow-net` flag.</h2>
</div>
<CodeMirror
  class="editor"
  placeholder={"Write your Deno code here"}
  bind:value
  lang={javascript({ typescript: true })}
  theme={oneDark}
  styles={{
    "&": {
      width: "1000px",
      maxWidth: "100%",
      height: "auto",
      margin: "0 auto",
      fontSize: "1.1rem",
    },
  }}
/>

<div class="center">
  <button on:click={async () => await execute(value)}>Run</button>
</div>

<div class="center">
  <div class="timer"></div>
  <h2>Output</h2>
</div>

<div class="output">
  <pre>
    <code>
 // Output will be displayed here
    </code>
  </pre>
</div>

<footer>
  <div class="center">
    <p>
      Made with ❤️ by
      <a href="https://github.com/paij0se" target="_blank">paij0se</a>
    </p>
    <p>
      <a href="/faq">FAQ</a>
    </p>
  </div>
</footer>

<style>
  @import "../../style.css";
</style>

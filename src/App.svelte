<script>
  import ActionBar from "./components/action-bar/action-bar.svelte";
  import AppBar from "./components/action-bar/app-bar.svelte";
  import Editor from "./components/editor/editor.svelte";
  import Player from "./components/player/player.svelte";

  let lang = "javascript";
  let typingSpeed = 50;
  let origineCodeContent = `let x = 5; let y = 6; let z = x + y;`;
  let _player;
  let playing;

  const play = () => {
    _player.writeCode(origineCodeContent);
  };
  const pause = () => {
    _player.suspendCodeTyping();
  };
  const stop = () => {
    _player.stop();
  };
  const clearCode = () => {
    _player.clearCode();
  };
</script>

<AppBar bind:selectedLang={lang} />

<div class="editors">
  <div style="width: 40vw; min-width: 270px;">
    <Editor language={lang} bind:codeContent={origineCodeContent} />
  </div>
  
  <div style="width: 60vw; min-width: 520px;">
    <ActionBar on:play={play} on:stop={stop} on:pause={pause} on:clear={clearCode} bind:typingSpeed={typingSpeed} bind:isPlaying={playing} />
    <Player language={lang} bind:this={_player} bind:typingSpeed={typingSpeed} bind:playing={playing}/>
  </div>
</div>

<style global>

  :root {
    min-width: 790px;
  }
  .editors {
    display: flex;
  }
</style>

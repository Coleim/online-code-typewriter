<script>
  import ActionBar from "./components/action-bar/action-bar.svelte";
  import AppBar from "./components/action-bar/app-bar.svelte";
  import Editor from "./components/editor/editor.svelte";
  import Player from "./components/player/player.svelte";

  let lang = "javascript";
  let typingSpeed = 10;
  let origineCodeContent = `let x = 5; let y = 6; let z = x + y;`;
  let _player;

  const play = () => {
    _player.writeCode(origineCodeContent);
  };
  const stop = () => {
    _player.clearCode();
  };
</script>


<AppBar bind:selectedLang={lang} />

<div class="editors">
  <div style="width: 40vw">
    <Editor language={lang} bind:codeContent={origineCodeContent} />
  </div>
  
  <div style="width: 60vw">
    <ActionBar on:play={play} on:stop={stop} bind:typingSpeed={typingSpeed}/>
    <!-- <ResizeVerticalBar /> -->
    <Player language={lang} bind:this={_player} bind:typingSpeed={typingSpeed}/>
  </div>
</div>

<style global>
  .editors {
    display: flex;
  }
</style>

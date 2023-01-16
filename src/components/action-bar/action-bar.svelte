<script>
    import { createEventDispatcher } from 'svelte';
    import TopAppBar, { Row, Section } from "@smui/top-app-bar";
    import IconButton from "@smui/icon-button";
    import Slider from '@smui/slider';

    export let typingSpeed;
    export let isPlaying;
 
    let isPaused = true;

	const dispatch = createEventDispatcher();
    const play = () => {
		dispatch('play');
        isPaused = false;
	}
    const pause = () => {
		dispatch('pause');
        isPaused = true;
	}
    const stop = () => {
		dispatch('stop');
        isPaused = false;
	}
    const clear = () => {
		dispatch('clear');
        isPaused = false;
	}

</script>

<TopAppBar variant="static" color="secondary" dense>
    <Row>
        <Section toolbar align="start">
            {#if !isPlaying || isPaused}
                <IconButton class="material-icons" on:click={play}>play_arrow</IconButton>
            {:else}
                <IconButton class="material-icons" on:click={pause}>pause</IconButton>
            {/if}
            <IconButton class="material-icons" on:click={stop}>stop</IconButton>
            <!-- <IconButton class="material-icons" on:click={clear}>playlist_remove</IconButton>     -->
            <IconButton class="material-icons" on:click={clear}>delete_sweep</IconButton>
        </Section>

        <Section toolbar>
            <div style="width: 300px; display: flex; align-items: center">
                <span style="">Typing speed:</span>
                <Slider style="flex-grow: 1;"
                    bind:value={typingSpeed}
                    min={0}
                    max={100}
                    step={1}
                    input$aria-label="Typing speed"
                />
                { typingSpeed }
            </div>
        </Section>


    </Row>
</TopAppBar>

<script>
    import { createEventDispatcher } from 'svelte';
    import TopAppBar, { Row, Section } from "@smui/top-app-bar";
    import IconButton from "@smui/icon-button";
    import Slider from '@smui/slider';
    import FormField from '@smui/form-field';

    export let typingSpeed = 10;

	const dispatch = createEventDispatcher();
    const play = () => {
		dispatch('play');
	}
    const stop = () => {
		dispatch('stop');
	}



</script>

<TopAppBar variant="static" color="secondary" dense>
    <Row>
        <Section toolbar align="start">
            <!-- <IconButton class="material-icons">file_open</IconButton> -->
            <IconButton class="material-icons" on:click={play}>play_arrow</IconButton>
            <!-- <IconButton class="material-icons">pause</IconButton> -->
            <IconButton class="material-icons" on:click={stop}>stop</IconButton>         
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

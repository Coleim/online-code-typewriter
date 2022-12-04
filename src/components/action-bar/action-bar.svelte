<script>
    import { createEventDispatcher } from 'svelte';
    import TopAppBar, { Row, Section } from "@smui/top-app-bar";
    import IconButton from "@smui/icon-button";

	const dispatch = createEventDispatcher();
    const play = () => {
		dispatch('play');
	}
    const stop = () => {
		dispatch('stop');
	}



</script>

<TopAppBar variant="static" color="primary" dense>
    <Row>
        <Section toolbar align="start">
            <IconButton class="material-icons">file_open</IconButton>
            <IconButton class="material-icons" on:click={play}>play_arrow</IconButton>
            <IconButton class="material-icons">pause</IconButton>
            <IconButton class="material-icons" on:click={stop}>stop</IconButton>
        </Section>
    </Row>
</TopAppBar>

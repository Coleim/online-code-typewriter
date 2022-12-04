<script>
    import { createEventDispatcher } from 'svelte';
    import TopAppBar, { Row, Section, Title } from "@smui/top-app-bar";
    import Select, { Option } from "@smui/select";
    import IconButton from "@smui/icon-button";
    import * as monaco from "monaco-editor";

    let availableLanguages = monaco.languages.getLanguages();
    export let selectedLang = 'typescript'; // availableLanguages.at(0).id;

</script>

<TopAppBar variant="static" color="primary" dense>
    <Row>
        <Section toolbar align="start">
            <Select bind:value={selectedLang} label="Selected Language">
                {#each availableLanguages as lang}
                    <Option value={lang.id}>
                        {#if lang.aliases?.length > 0}
                            {lang.aliases.at(0)}
                        {:else}
                            {lang.id}
                        {/if}
                    </Option>
                {/each}
            </Select>
        </Section>

        <Section>
            <Title>Online Code Typewriter</Title>
        </Section>

        <Section align="end">
            <IconButton class="material-icons">settings</IconButton>
        </Section>
    </Row>
</TopAppBar>

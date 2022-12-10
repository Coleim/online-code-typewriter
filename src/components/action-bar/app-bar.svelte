<script lang="ts">
    import TopAppBar, { Row, Section, Title } from "@smui/top-app-bar";
    import IconButton from "@smui/icon-button";
    import Menu from '@smui/menu';
    import List, { Item, Text } from '@smui/list';
    import Button, { Label } from '@smui/button';
    import * as monaco from "monaco-editor";

    export let selectedLang;

    let availableLanguages = monaco.languages.getLanguages();
    let selectedLanguage = availableLanguages.at(0);
    let menu: Menu;

    $: { selectedLang = selectedLanguage.id }

    let lastUsedLangs: monaco.languages.ILanguageExtensionPoint[] = JSON.parse(localStorage.getItem("lastUsedLangs"));
    if(lastUsedLangs === null) {
        lastUsedLangs = [];
    }
    while(lastUsedLangs.length > 3) {
        lastUsedLangs.shift();
    }
    if(lastUsedLangs.length > 0) {
        selectedLanguage = lastUsedLangs.at(-1);
    }

    const selectLang = (lang) => {
        selectedLanguage = lang;
        if(lastUsedLangs.length >= 3) {
            lastUsedLangs.shift();
        }
        lastUsedLangs = [...lastUsedLangs, selectedLanguage];
        localStorage.setItem("lastUsedLangs", JSON.stringify(lastUsedLangs));
        menu.setOpen(false);
    }

</script>

<TopAppBar variant="static" color="primary" dense >
    <Row>
        <Section toolbar align="start" >
            <Button on:click={() => menu.setOpen(true)}>
                <Label>
                    {#if selectedLanguage.aliases?.length > 0}
                        {selectedLanguage.aliases.at(0)}
                    {:else}
                        {selectedLanguage.id}
                    {/if}
                </Label>
            </Button>
            <Menu bind:this={menu}>
                <List>
                    {#each availableLanguages as lang}
                        <Item on:SMUI:action={ () => selectLang(lang) }>
                            <Text>
                                {#if lang.aliases?.length > 0}
                                    {lang.aliases.at(0)}
                                {:else}
                                    {lang.id}
                                {/if}
                            </Text>
                        </Item>
                    {/each}
                </List>
            </Menu>
        </Section>

        
        <Section>
            {#each lastUsedLangs.slice(0, lastUsedLangs.length-1).reverse() as lang}
                <Button on:click={() => selectLang(lang) }>
                    <Label>
                        {#if lang.aliases?.length > 0}
                            {lang.aliases.at(0)}
                        {:else}
                            {lang.id}
                        {/if}
                    </Label>
                </Button>
            {/each}
        </Section>

        <Section>
            <Title>Online Code Typewriter</Title>
        </Section>

        <Section align="end">
            <IconButton class="material-icons">settings</IconButton>
            <IconButton class="material-icons">help</IconButton>
        </Section>
    </Row>
</TopAppBar>


<style global>

    .mdc-select__selected-text,
    .mdc-floating-label--float-above,
    .mdc-select__dropdown-icon,
    .mdc-select__dropdown-icon-graphic,
    .mdc-select__dropdown-icon-active,
    .mdc-select__dropdown-icon-inactive {
      color: #fff !important;
    }

    .mdc-line-ripple::before, .mdc-line-ripple::after {
        border-bottom-style: none;
    }
  </style>
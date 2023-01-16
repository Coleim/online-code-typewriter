<script lang="ts">
    import TopAppBar, { Row, Section, Title } from "@smui/top-app-bar";
    import IconButton, { Icon } from "@smui/icon-button";
    import { Svg } from '@smui/common';
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
            <IconButton href="https://github.com/Coleim/online-code-typewriter" target="_blank">
                <Icon component={Svg} viewBox="0 0 24 24">
                    <path d="M12 0c-6.626 0-12 5.373-12 12 0 5.302 3.438 9.8 8.207 11.387.599.111.793-.261.793-.577v-2.234c-3.338.726-4.033-1.416-4.033-1.416-.546-1.387-1.333-1.756-1.333-1.756-1.089-.745.083-.729.083-.729 1.205.084 1.839 1.237 1.839 1.237 1.07 1.834 2.807 1.304 3.492.997.107-.775.418-1.305.762-1.604-2.665-.305-5.467-1.334-5.467-5.931 0-1.311.469-2.381 1.236-3.221-.124-.303-.535-1.524.117-3.176 0 0 1.008-.322 3.301 1.23.957-.266 1.983-.399 3.003-.404 1.02.005 2.047.138 3.006.404 2.291-1.552 3.297-1.23 3.297-1.23.653 1.653.242 2.874.118 3.176.77.84 1.235 1.911 1.235 3.221 0 4.609-2.807 5.624-5.479 5.921.43.372.823 1.102.823 2.222v3.293c0 .319.192.694.801.576 4.765-1.589 8.199-6.086 8.199-11.386 0-6.627-5.373-12-12-12z"/>
                </Icon>
              </IconButton>
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
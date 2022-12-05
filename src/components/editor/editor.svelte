<script lang="ts">
    import loader from "@monaco-editor/loader";
    import type * as monaco from "monaco-editor";
    import godotColors from './godot-colors.json';

    export let language: string = 'typescript';
    export let codeContent: string = '';
    // export let theme: string = 'vs-dark';
    // export let theme: string = 'vs';
    export let theme: string = 'vs-dark';
    export let automaticLayout: boolean = true;
    export let autoClosingBrackets: monaco.editor.EditorAutoClosingStrategy = "languageDefined";
    export let autoIndent: 'none' | 'keep' | 'brackets' | 'advanced' | 'full' = "none";
    export let autoSurround: monaco.editor.EditorAutoSurroundStrategy= "never";
    export let quickSuggestions: boolean = false;
    export let readOnly: boolean = false;
    export let snippetSuggestions: 'top' | 'bottom' | 'inline' | 'none' = "none";
    export let minimap: boolean = true;

    export const typeCode = (text) => {
        if(_codeEditor) {
            _codeEditor.trigger('keyboard', 'type', { text })
        }
    }
    export const clearCode = () => {
        if(_codeEditor) {
            _codeEditor.setValue("");
        }
    }

    let divEl;
    let _codeEditor: monaco.editor.IStandaloneCodeEditor;
    let _model;
    let _monaco;

    loader.init().then((m) => {
        _monaco = m;
        _model = _monaco.editor.createModel(codeContent, language);
        _codeEditor = _monaco.editor.create(divEl, {
            model: _model,
            theme: theme,
            automaticLayout: automaticLayout,
            autoClosingBrackets: autoClosingBrackets,
            autoIndent: autoIndent,
            autoSurround: autoSurround,
            quickSuggestions: quickSuggestions,
            readOnly: readOnly,
            snippetSuggestions: snippetSuggestions,
            minimap: {
                enabled: minimap
            }
        });
        _model.onDidChangeContent(() => {
            codeContent = _codeEditor.getValue();
        });
        _monaco.languages.typescript.typescriptDefaults.setDiagnosticsOptions({
            noSemanticValidation: true,
            noSyntaxValidation: true,
        });
        _monaco.languages.typescript.javascriptDefaults.setDiagnosticsOptions({
            noSemanticValidation: true,
            noSyntaxValidation: true,
        });

        defineThemes();

    });
    
    const updateLanguage = (lang) => {
        if(_monaco) {
            _monaco.editor.setModelLanguage(_monaco.editor.getModel(_model.uri), lang);
        }
    } 

    $: updateLanguage(language);
    $: { _codeEditor?.updateOptions({ theme: theme }); updateBackgroundColor() }
    $: { _codeEditor?.updateOptions({ automaticLayout: automaticLayout }) }
    $: { _codeEditor?.updateOptions({ autoClosingBrackets: autoClosingBrackets }) }
    $: { _codeEditor?.updateOptions({ autoIndent: autoIndent }) }
    $: { _codeEditor?.updateOptions({ autoSurround: autoSurround }) }
    $: { _codeEditor?.updateOptions({ quickSuggestions: quickSuggestions }) }
    $: { _codeEditor?.updateOptions({ readOnly: readOnly }) }
    $: { _codeEditor?.updateOptions({ snippetSuggestions: snippetSuggestions }) }
    $: { _codeEditor?.updateOptions({ minimap: { enabled: minimap} }) }

    $: { const currentValue = _codeEditor?.getValue(); if(currentValue !== codeContent) { _codeEditor?.setValue(codeContent) } }



    function defineThemes() {
        _monaco.editor.defineTheme("godot-theme", {
            base: "vs-dark", // can also be vs-dark or hc-black
            inherit: true, // can also be false to completely replace the builtin rules
            rules: [
                {
                token: "comment",
                foreground: "#72767f",
                // fontStyle: "italic",
                },
                { token: "comment.js", foreground: "008800", fontStyle: "bold" },
                { token: "comment.css", foreground: "0000ff" }, // will inherit fontStyle from `comment` above
            ],
            colors: godotColors
        });        
    }


    function updateBackgroundColor() {
        switch(theme) {
            case 'godot-theme':
            document.documentElement.style.cssText = `--container-background:${godotColors["editor.background"]}`;
            break;
            case 'vs-dark': 
            document.documentElement.style.cssText = `--container-background: #1e1e1e`;
            break;
            case 'vs': 
            document.documentElement.style.cssText = `--container-background: #fffffe`;
            break;
        }       
        
    }
</script>

<div bind:this={divEl} class="editor" />

<style>
    :root {
        background-color: var(--container-background);
    }
    .editor {
        margin-top: 20px;
        height: 80vh;
    }
</style>
<script lang="ts">
    import Editor from "../editor/editor.svelte";
    export let language;
    let _editor;
    let _typingIntervalId;
    let _playing = false;

    export const writeCode = (codeToType) => {
        if(_playing) return;
        codeToType = codeToType.replaceAll('\r\n', '\n');
        _playing = true;
        let index = 0;
        _typingIntervalId = setInterval(() => {
            const writtenCode = codeToType.charAt(index);
            _editor.typeCode(writtenCode);
            ++index;
            if(index >= codeToType.length) {
                stop();
                _playing = false;
            }
        }, 40);
    }

    export const clearCode = () => {
        stop();
        _editor.clearCode();
    }

    const stop = () => {
        clearInterval(_typingIntervalId);
        _playing = false;
    }
    
</script>

<Editor bind:this={_editor} language={language}
    autoIndent={'none'} 
    autoClosingBrackets={'never'} />



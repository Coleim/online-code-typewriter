<script lang="ts">
    import Editor from "../editor/editor.svelte";
    export let language;
    export let typingSpeed;
    export let playing = false;
    let _editor;
    let _typingIntervalId;
    let _isPaused = false;

    export const writeCode = (codeToType) => {
        _isPaused = false;
        if(playing) return;
        codeToType = codeToType.replaceAll('\r\n', '\n');
        playing = true;
        let index = 0;
        _typingIntervalId = setInterval(() => {
            console.log("_isPaused: " , _isPaused)
            console.log("playing: " , playing)
            if(!_isPaused) {
                const writtenCode = codeToType.charAt(index);
                console.log("writtenCode: " , writtenCode)
                _editor.typeCode(writtenCode);
                ++index;
                if(index >= codeToType.length) {
                    stop();
                }
            }
        }, 100-typingSpeed);
        console.log("_typingIntervalId: ", _typingIntervalId)

    }

    export const stopTyping = () => {
        stop();
    }

    export const clearCode = () => {
        stop();
        _editor.clearCode();
    }

    export const suspendCodeTyping = () => {
        _isPaused = true;
    }

    export const stop = () => {
        console.log("stop: ", _typingIntervalId)
        clearInterval(_typingIntervalId);
        playing = false;
    }
    
</script>

<Editor bind:this={_editor} language={language}
    autoIndent={'none'}
    minimap={false}
    autoClosingBrackets={'never'} />



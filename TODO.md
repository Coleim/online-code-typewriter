<script lang="ts">
  import { onMount } from "svelte";
  import loader from "@monaco-editor/loader";
  import * as monaco from "monaco-editor";

  let divEl: HTMLDivElement = null;
  let editor: monaco.editor.IStandaloneCodeEditor;

  // Playground: https://microsoft.github.io/monaco-editor/playground.html#customizing-the-appearence-tokens-and-colors
  // Theme colors: https://gitlab.com/JamesSauer/GDScript-Theme-for-VSCode/-/blob/master/themes/GDScript-color-theme.json
  // Regex: https://github.com/GodotID/nano-gdscript/blob/main/gdscript.nanorc
  // Regex 2 : https://github.com/godotengine/godot-vscode-plugin/blob/master/syntaxes/GDScript.tmLanguage.json
  
  loader.init().then((monaco) => {
    monaco.languages.register({ id: "gdscript" });
    monaco.languages.setMonarchTokensProvider("gdscript", {
      tokenizer: {
        root: [
          [/#.*/, "comment"],
          [/\[notice.*/, "custom-notice"],
          [/\[info.*/, "custom-info"],
          [/\[[a-zA-Z 0-9:]+\]/, "custom-date"],
        ],
      },
    });

    monaco.editor.defineTheme("myCustomTheme", {
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
      colors: {
        "editor.background": "#202531",
        "editor.foreground": "#eeffff",
        "activityBarBadge.background": "#007acc",
        "activityBar.background": "#333B4F",
        "sideBarTitle.foreground": "#bbbbbb",
        "editorGroupHeader.tabsBackground": "#262C3B",
        "editorHoverWidget.background": "#262C3B",
        "editorSuggestWidget.background": "#262C3B",
        "editorWidget.background": "#262C3B",
        "editor.lineHighlightBorder": "#202531",
        "notifications.background": "#262C3B",
        "sideBar.background": "#262C3B",
        "statusBar.background": "#262C3B",
        "statusBar.debuggingBackground": "#262C3B",
        "statusBar.noFolderBackground": "#262C3B",
        "tab.inactiveBackground": "#262C3B",
        //"breadcrumbPicker.background": "#262C3B",
        //"quickInput.background": "#262C3B",
        "sideBarSectionHeader.background": "#333B4F",
        "titleBar.activeBackground": "#333B4F",
        "titleBar.inactiveBackground": "#333B4F",
        //"breadcrumb.background": "#333B4F",
        //"editorGroupHeader.noTabsBackground": "#333B4F",
        //"editorGutter.background": "#333B4F",
        //"editorHoverWidget.statusBarBackground": "#333B4F",
        //"editorPane.background": "#333B4F",
        //"panel.background": "#333B4F",
        //"terminal.background": "#333B4F",
        "dropdown.background": "#333B4F",
        "input.background": "#333B4F",
        //"checkbox.background": "#333B4F",
        //"menu.background": "#333B4F",
        //"settings.checkboxBackground": "#333B4F",
        //"settings.dropdownBackground": "#333B4F",
        //"settings.numberInputBackground": "#333B4F",
        //"settings.textInputBackground": "#333B4F",
        "editorSuggestWidget.selectedBackground": "#515662",
        "list.activeSelectionBackground": "#515662",
        "list.inactiveSelectionBackground": "#515662",
        "list.hoverBackground": "#333b4f",
        "statusBarItem.hoverBackground": "#515662",
        //"menu.selectionBackground": "#515662",
        "gitDecoration.conflictingResourceForeground": "#ffcb6b90",
        "gitDecoration.deletedResourceForeground": "#ff537090",
        "gitDecoration.ignoredResourceForeground": "#5f7a8790",
        "gitDecoration.modifiedResourceForeground": "#82aaff90",
        "gitDecoration.untrackedResourceForeground": "#c3e88d90",
        "peekViewResult.background": "#202531",
        "peekViewTitle.background": "#202531",
      }
    });

    editor = monaco.editor.create(divEl, {
      value: `# Comment 
      #### Comment lol

      Not a # comment 
      for i in get_slide_count():
  var collision = get_slide_collision(i)
  if is_on_floor() and collision.collider is TileMap and collision.normal == Vector2(0, -1):
    var tile_pos = collision.collider.world_to_map(position)
    tile_pos -= collision.normal
    var tile_id = collision.collider.get_cellv(tile_pos)
    if tile_id != -1 and tile_pos != previousCollision:
      previousCollision = tile_pos				
      if !litTiles.has(tile_pos):
        var light = templateLight.duplicate()
        var tile_center_pos = collision.collider.map_to_world(tile_pos) + Vector2(14, 3)
        light.position = tile_center_pos
        light.show()
        get_parent().add_child(light)
        litTiles[tile_pos] = "ON"
        emit_signal("tile_cleaned")`,
      theme: "myCustomTheme",
      language: "gdscript",
      autoClosingBrackets: "never",
      autoIndent: "none",
      // readOnly: true
    });
  });
</script>

<div bind:this={divEl} style="min-height: 500px;" />

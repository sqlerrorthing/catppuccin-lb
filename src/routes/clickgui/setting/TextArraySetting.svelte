<script lang="ts">
    import type {ModuleSetting, TextArraySetting} from "../../../integration/types";
    import {convertToSpacedString, spaceSeperatedNames} from "../../../theme/theme_config";
    import {createEventDispatcher} from "svelte";

    export let setting: ModuleSetting;

    const cSetting = setting as TextArraySetting;

    const dispatch = createEventDispatcher();

    function handleChange() {
        setting = {...cSetting};
        dispatch("change");
    }

    function removeValueIndex(index: number) {
        cSetting.value.splice(index, 1);
        cSetting.value = cSetting.value;
        handleChange();
    }

    function addValueIndex() {
        cSetting.value = ["", ...cSetting.value];
        handleChange();
    }
</script>

<div class="setting">
    <div class="name">{$spaceSeperatedNames ? convertToSpacedString(cSetting.name) : cSetting.name}</div>
    <button class="button-add" on:click={addValueIndex}>Add value</button>
    {#if cSetting.value.length > 0}
        <div class="inputs">
            {#each cSetting.value as _, index}
                <div class="input-wrapper">
                    <input type="text" class="value" spellcheck="false" placeholder={setting.name} bind:value={cSetting.value[index]}
                           on:input={handleChange}>
                    <button class="button-remove" title="Remove" on:click={() => removeValueIndex(index)}>
                        <img src="img/clickgui/icon-cross.svg" alt="remove">
                    </button>
                </div>
            {/each}
        </div>
    {/if}
</div>

<style lang="scss">
  @use "sass:color";
  @use "../../../colors.scss" as *;

  .input-wrapper {
    display: grid;
    grid-template-columns: 1fr max-content;
    column-gap: 5px;
    align-items: center;
  }

  .button-remove {
    background-color: transparent;
    border: none;
    cursor: pointer;
  }

  .setting {
    padding: 7px 0px;
  }

  .inputs {
    display: flex;
    flex-direction: column;
    row-gap: 10px;
    margin-top: 5px;
  }

  .name {
    font-weight: 500;
    color: $text;
    font-size: 12px;
    margin-bottom: 5px;
  }

  .button-add {
    font-family: monospace;
    font-size: 12px;
    color: $text;
    background-color: $accent;
    border: none;
    padding: 6px 10px;
    border-radius: 3px;
    width: 100%;
    cursor: pointer;
    transition: ease background-color .2s;

    &:hover {
        background-color: color.adjust(color.adjust($accent, $saturation: -30%), $lightness: -10%);
    }
  }

  .value {
    width: 100%;
    background-color: rgba($crust, .5);
    border: 1px solid rgba($text, 0.1);
    border-left: 2px solid $accent;
    font-size: 12px;
    color: $text;
    border: none;
    padding: 6px;
    border-radius: 3px;
    transition: ease border-color .2s;

    &::-webkit-scrollbar {
      background-color: transparent;
    }
  }
</style>

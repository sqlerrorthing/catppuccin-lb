<script lang="ts">
    export let maxHealth: number;
    export let health: number;

    let width = Math.ceil(health / maxHealth * 100);
    $: width = Math.ceil(health / maxHealth * 100);
</script>

<div class="health-progress">
    <div class="thumb" style="width: {width}%;"></div>
</div>

<style lang="scss">
    @use "../../../../colors.scss" as *;

    .health-progress {
        position: relative;
        padding: 12px;
    }

    .thumb {
        height: 8px;
        background-color: $hotbar-health-color;
        transition: ease-out width 0.5s;
        border-radius: 8px;
    }
</style>
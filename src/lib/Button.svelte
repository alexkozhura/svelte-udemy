<script>
    export let size = "large";
    export let shadow = false;
    // if unspecified, inherits from the root
    export let bgColor = "inherit";
    export let textColor = "inherit";
</script>

<button 
    style:--buttonBgColor={bgColor}
    style:--buttonTextColor={textColor}
    class:size-large={size === "large"}
    class:size-small={size==="small"}
    class:shadow
>
    <div class="left-content">
        <slot name="leftContent"></slot>
    </div>
    <slot>Fallback text</slot>
</button>

<style lang="scss">
    button {
        display: flex;
        align-items: center;
        border: none;
        background-color: var(--buttonBgColor);
        color: var(--buttonTextColor);
        transition: filter 0.3s ease;
        
        font-weight: bold;
        border-radius: 5px;
        cursor: pointer;
        .left-content {
            margin-right: 10px;
        }
        &.size-small {
            padding: 15px 20px;
        }
        &:hover {
            filter: brightness(85%);
        }
        &:active {
            filter: brightness(115%);
        }
        &.size-large {
            padding: 20px 25px;
            font-size: 20px;
        }
        &.shadow {
            box-shadow: 0 0 10px rgba(1,1,1,0.3);
        }
    }
</style>
<template>
    <div class="checkbox">
        <label class="cursor-pointer">
            <input type="checkbox"
                   :checked="value"
                   @change="$emit('input', !value)">
            <span class="cr"><i class="cr-icon fas fa-check"></i></span>
            {{ text }}
            <slot></slot>
        </label>
    </div>
</template>

<script>
    export default {
        props: ['text', 'value']
    }
</script>

<style scoped>
    .checkbox label:after {
        content: '';
        display: table;
        clear: both;
    }

    .checkbox .cr {
        position: relative;
        display: inline-block;
        border: 1px solid #a9a9a9;
        border-radius: .25em;
        width: 1.3em;
        height: 1.3em;
        float: left;
        margin-right: .5em;
    }

    .checkbox .cr .cr-icon {
        position: absolute;
        font-size: .8em;
        line-height: 0;
        top: 50%;
        left: 20%;
    }

    .radio .cr .cr-icon {
        margin-left: 0.04em;
    }

    .checkbox label input[type="checkbox"] {
        display: none;
    }

    .checkbox label input[type="checkbox"] + .cr > .cr-icon {
        transform: scale(3) rotateZ(-20deg);
        opacity: 0;
    }

    .checkbox label input[type="checkbox"]:checked + .cr > .cr-icon {
        transform: scale(1) rotateZ(0deg);
        opacity: 1;
    }

    .checkbox label input[type="checkbox"]:disabled + .cr {
        opacity: .5;
    }
</style>

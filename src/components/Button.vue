<template>
    <button @click="onClick()"
            v-bind:style="{ background: color }" 
            class="btn">{{ btnText }}</button>
</template>

<script>
export default {
    name: 'Button',
    props: {
        btnText: String,
        color: String,
    },
    methods: {
        onClick() {
            console.log('click');
        }
    }
}
</script>

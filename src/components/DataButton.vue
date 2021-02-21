<template>
    <button class="btn"
            @click="handler">Get data</button>
</template>

<script>
export default {
    name: 'DataButton',
    props: {
        handler: {
            type: Function,
            required: false
        }
    }
}
</script>

<style></style>

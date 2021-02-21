<template>
    <div class="loading" v-if="show">
        <strong>Loading...</strong>
    </div>
</template>

<script>
export default {
    name: 'Loader',
    props: {
        show: {
            type: Boolean,
            required: true,
            default: false
        }
    }
}
</script>

<style></style>

<script setup>
// eslint-disable-next-line no-undef
const props = defineProps({
    value: String,
});

</script>
<template>
    <img :src="props.value" />
</template>
<style scoped>
img{
    width: 50px;
    height: 50px;
    border-radius: 50%;
}
</style>
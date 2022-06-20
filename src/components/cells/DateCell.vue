<script setup>
import { computed } from 'vue';
// eslint-disable-next-line no-undef
const props = defineProps({
    value: String,
});
const formated = computed(() => {
    const d = new Date(props.value);
    return `${d.getDate()} ${d.toLocaleString('default', { month: 'short' })} ${d.getFullYear()}`;
});
</script>
<template>
    {{ formated }}
</template>
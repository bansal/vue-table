<script setup>
// eslint-disable-next-line no-undef
const props = defineProps({
    value: String,
});
</script>
<template>
   <a :href="`mailto:${props.value}`">{{ props.value }}</a>
</template>
<script setup>
// eslint-disable-next-line no-undef
const props = defineProps({
    value: Object,
});
</script>
<template>
   <a :href="`https://www.google.com/maps/search/?api=1&query=${props.value.latitude}%2C${props.value.longitude}`">
    {{ props.value.number}}, {{ props.value.name}}, <br />
    {{ props.value.city}}, {{ props.value.state}}
   </a>
</template>
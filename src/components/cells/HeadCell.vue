<script setup>
import UpArrowIcon from '../icons/UpArrowIcon.vue';
import DownArrowIcon from '../icons/DownArrowIcon.vue';
// eslint-disable-next-line no-undef
const props = defineProps({
  sortable: Boolean,
  active: Boolean,
  asc: Boolean,
});

// eslint-disable-next-line no-undef
const emit = defineEmits(['sort']);

</script>
<template>
    <th>
        <div
            :class="`head-cell sortable ${props.active ? 'active' : ''}`"
            v-if="props.sortable"
            @click="emit('sort')"
        >
            <slot />
            <UpArrowIcon class="icon" v-if="props.asc" />
            <DownArrowIcon class="icon" v-else />
        </div>
        <div class="head-cell" v-else>
            <slot />
        </div>
    </th>
</template>
<style scoped>
th{
    padding: 0px;
}
.head-cell{
    padding: 20px;
    font-weight: bold;
    display: flex;
    justify-content: flex-start;
    align-items: center;
    gap: 10px;
    cursor: default;
}

.sortable {
    cursor: pointer;
}
.sortable .icon {
   visibility: hidden;
}
.sortable:hover .icon,
.active .icon {
    visibility: visible;
}
.sortable:hover,
.active{
    color: var(--table-header-active-text-color);
    background-color: var(--table-header-active-bg-color);
}
</style>
<script setup>
// eslint-disable-next-line no-undef
const emit = defineEmits(['prevpage', 'nextpage', 'perpage']);

// eslint-disable-next-line no-undef
const props = defineProps({
  perPage: Number,
  currentPage: Number,
  totalPages: Number,
});

const onSelectChange = (e) => {
    emit('perpage', parseInt(e.target.value, 10));
}

</script>
<template>
    <div class="pagination">
        <span>Rows per page</span>
        <select :value="props.perPage" @change="onSelectChange">
            <option value="5">5</option>
            <option value="10">10</option>
            <option value="25">25</option>
            <option value="50">50</option>
            <option value="0">All</option>
        </select>
        <button
            type="button"
            :disabled="props.currentPage <= 1"
            @click="emit('prevpage')"
        >&lt; Prev</button>
        <div>{{ props.currentPage}} of {{ props.totalPages }}</div>
        <button
            type="button"
            :disabled="props.currentPage >= props.totalPages"
            @click="emit('nextpage')"
        >Next &gt;</button>
    </div>
</template>
<style scoped>
.pagination{
    display: flex;
    justify-content: flex-end;
    align-items: center;
    gap: 10px;
    padding: 15px 20px;
    background-color: var(--table-header-bg-color);
    color: var(--table-header-text-color);
    border-top: 1px solid var(--table-border-color);
}

button,
select {
    background-color: var(--table-header-active-bg-color);
    color: var(--table-header-active-text-color);
    border: none;
    padding: 5px 10px;
    border-radius: 4px;
    cursor: pointer;
}
button:hover:not(:disabled),
select:hover {
    background-color: var(--button-hover-bg-color);
    color: var(--button-hover-text-color);
}
button:disabled {
    opacity: 0.5;
    cursor: not-allowed;
}
</style>
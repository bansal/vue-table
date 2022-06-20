<script setup>
import { computed, reactive } from 'vue';
import AddressCell from './cells/AddressCell.vue';
import AvatarCell from './cells/AvatarCell.vue';
import Cell from './cells/Cell.vue';
import DateCell from './cells/DateCell.vue';
import EmailCell from './cells/EmailCell.vue';
import HeadCell from './cells/HeadCell.vue';
import Pagination from './Pagination.vue';
import Row from './Row.vue';
import TableHead from './TableHead.vue';
import TelCell from './cells/TelCell.vue';

const cellType = {
    avatar: AvatarCell,
    address: AddressCell,
    email: EmailCell,
    tel: TelCell,
    date: DateCell,
}
// eslint-disable-next-line no-undef
const props = defineProps({
  header: Array,
  data: Array,
  loading: Boolean,
  error: String,
});

const state = reactive({
    sort:{
        field: 'slno',
        direction: 'desc'
    },
    perPage: 10,
    currentPage: 1,
});

const sorted = computed(() => {
    const { data } = props;
    const { field, direction } = state.sort;
    return data.sort((a, b) => {
    return direction === 'asc'
        ? a[field] < b[field]
        ? -1
        : a[field] > b[field]
        ? 1
        : 0
        : a[field] > b[field]
        ? -1
        : a[field] < b[field]
        ? 1
        : 0;
    });
})

const page = computed(() => {
    const { perPage, currentPage } = state;
    return sorted.value.slice((currentPage - 1) * perPage, currentPage * perPage);
});

const updateOrder = (field) => {
    const { sort } = state;
    if (sort.field === field) {
        state.sort.direction = sort.direction === 'desc' ? 'asc' : 'desc';
    } else {
        state.sort.field = field;
        state.sort.direction = 'desc';
    }
}

const prevPage = () => {
    if (state.currentPage > 1) {
        state.currentPage--;
    }
}
const nextPage = () => {
    if (state.currentPage < props.data.length / state.perPage) {
        state.currentPage++;
    }
}
const perPageChange = (perPage) => {
    if (perPage === 0) {
        state.perPage = props.data.length;
    } else {
        state.perPage = perPage;
    }
    state.currentPage = 1;
}
</script>
<template>
    <div>
        <table>
           <TableHead>
                <HeadCell
                    v-for="item in header"
                    :key="item.value"
                    :sortable="item.sortable"
                    :active="item.value === state.sort.field"
                    :asc="state.sort.direction === 'asc'"
                    @sort="updateOrder(item.value)"
                >
                    {{item.label}}
                </HeadCell>
            </TableHead>
            <tbody>
                <Row v-for="row in page" :key="row.slno">
                    <Cell v-for="item in header" :key="item.header">
                        <component
                            v-if="item.type"
                            :is="cellType[item.type]"
                            :value="row[item.value]"
                        />
                        <span v-else>{{row[item.value]}}</span>
                    </Cell>
                </Row>
            </tbody>
        </table>
        <Pagination
            :current-page="state.currentPage"
            :per-page="state.perPage"
            :total-pages="Math.ceil(props.data.length / state.perPage)"
            @perpage="perPageChange"
            @prevpage="prevPage"
            @nextpage="nextPage"
        />
    </div>
</template>
<style scoped>
table{
    width: 100%;
    border-collapse: collapse;
    border-spacing: 0;
    background-color: var(--table-bg-color);
}
</style>
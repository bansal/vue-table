<script setup>
import { onMounted, reactive, ref } from 'vue';
import Vuetable from './Vuetable.vue';
import SpinnerIcon from './icons/SpinnerIcon.vue';

const loading = ref(false);
const error = ref('');

const state = reactive({
    users: [],
});

const header = [
    { label: '#', value: 'slno', sortable: true },
    { label: 'Picture', value: 'picture', type: 'avatar' },
    { label: 'Name', value: 'name', sortable: true },
    { label: 'DoB', value: 'dob', type: 'date', sortable: true },
    { label: 'Email', value: 'email', type: 'email', sortable: true },
    { label: 'Location', value: 'location', type: 'address' },
    { label: 'Phone', value: 'phone', type: 'tel', sortable: true },
]
const flattenObject = (object) => {
    // flatten nested object with dot notation in key
    return Object.keys(object).reduce((result, key) => {
        const value = object[key];
        if (typeof value === 'object') {
            return { ...result, ...flattenObject(value) };
        }
        return { ...result, [key]: value };
    }, {});
}

const flattenData = (raw) => {
    let slno = 1;
    const flattened = raw.map((item) => {
        return {
            slno: slno++,
            name: `${item.name.title} ${item.name.first} ${item.name.last}`,
            dob: item.dob.date,
            email: item.email,
            location: flattenObject(item.location),
            phone: item.phone,
            picture: item.picture.thumbnail,
        }
    });
    return flattened
}

const fetchData = async () => {
    loading.value = true;
    try {
        const data = await fetch("https://randomuser.me/api/?results=50");
        const jsonData = await data.json();
        state.users = flattenData(jsonData.results);
    } catch (e) {
        error.value = e.message;
    } finally {
        loading.value = false;
    }
}

onMounted(() => {
    fetchData();
});
</script>

<template>
    <div class="table-wrapper">
        <div v-if="loading" class="loader">
            <SpinnerIcon />
        </div>
        <div v-if="error" class="error">{{error}}</div>
        <Vuetable v-else :header="header" :data="state.users" />
    </div>
</template>

<style scoped>
.table-wrapper {
    position: relative;
    width: 90%;
    margin: 50px auto;
    border-radius: 8px;
    overflow: hidden;
    background-color: var(--table-bg-color);
    border: 1px solid var(--table-border-color);
}
.loader {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    z-index: 100;
    background: rgba(255,255,255,0.5);
    display: flex;
    justify-content: center;
    align-items: center;
}
.error {
    width: 100%;
    height: 200px;
    display: flex;
    justify-content: center;
    align-items: center;
}
</style>
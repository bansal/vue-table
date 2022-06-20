<template>
    <td>
        <slot />
    </td>
</template>
<style scoped>
td{
    padding: 5px;
}
td:first-child{
    padding-left: 20px;
}
td:last-child{
    padding-right: 20px;
}
</style>
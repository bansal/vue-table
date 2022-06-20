<template>
    <tr>
        <slot />
    </tr>
</template>
<style scoped>
tr:not(:last-child){
    border-bottom: 1px solid var(--table-border-color);
}
</style>
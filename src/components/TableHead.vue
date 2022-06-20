<template>
    <thead>
        <tr>
            <slot />
        </tr>
    </thead>
</template>
<style scoped>
thead{
    background-color: var(--table-header-bg-color);
    border-bottom: 1px solid var(--table-border-color);
}
</style>
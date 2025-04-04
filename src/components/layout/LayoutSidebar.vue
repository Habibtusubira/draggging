<template>
    <aside class="lab-sidebar">
        <ul>
            <li @click="$emit('navigate', 'experiments')">Experiments</li>
            <li @click="$emit('navigate', 'instruments')">Instruments</li>
            <li @click="$emit('navigate', 'settings')">Settings</li>
        </ul>
    </aside>
</template>

<script setup></script>

<style scoped>
.lab-sidebar {
    width: 200px;
    background-color: #333;
    color: white;
    padding: 1rem;
}

.lab-sidebar ul {
    list-style: none;
    padding: 0;
}

.lab-sidebar li {
    padding: 10px;
    cursor: pointer;
    border-bottom: 1px solid #444;
}

.lab-sidebar li:hover {
    background-color: #444;
}
</style>

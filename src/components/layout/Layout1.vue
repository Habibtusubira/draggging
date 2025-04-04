<template>
    <div class="lab-layout">
        <LayoutHead />

        <div class="lab-body">
            <LayoutSidebar />

            <main class="lab-main">
                <!-- Dynamic content -->
                <slot />
            </main>
        </div>
    </div>
</template>

<script setup>
import LayoutHead from "@/components/layout/LayoutHead.vue";
import LayoutSidebar from "@/components/layout/LayoutSidebar.vue";
</script>

<style scoped>
.lab-layout {
    display: flex;
    flex-direction: column;
    height: 100vh;
}

.lab-body {
    display: flex;
    flex: 1;
    overflow: hidden;
}

.lab-main {
    flex: 1;
    padding: 20px;
    background-color: #f8f9fa;
    overflow-y: auto;
}
</style>

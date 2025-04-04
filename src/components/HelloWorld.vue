<!-- gap positioning tool -->
<template>
    <div class="puzzle-wrapper">
        <!-- Background with gaps -->
        <div class="puzzle-board">
            <img src="/assets/zenitsu.jpg" alt="Puzzle" class="puzzle-bg" />

            <!-- Gaps where cutouts go -->
            <div
                v-for="(gap, index) in gaps"
                :key="index"
                class="puzzle-gap"
                :style="gap.style"
                @drop="dropPiece(index, $event)"
                @dragover.prevent
            >
                <img
                    v-if="gap.filled"
                    :src="gap.filled.image"
                    class="puzzle-cutout"
                    draggable="false"
                />
            </div>
        </div>

        <!-- Debug Mode: Drag Cutouts to Find Positions -->
        <div class="debug-mode">
            <h3>🔧 Drag to Adjust Positions (For Debugging)</h3>
            <img
                v-for="(cutout, index) in debugCutouts"
                :key="cutout.id"
                :src="cutout.image"
                class="debug-cutout"
                :style="{ top: cutout.top + 'px', left: cutout.left + 'px' }"
                @mousedown="startDrag(index, $event)"
            />
        </div>

        <!-- Pile of cutouts to pick from -->
        <div class="cutouts-pile">
            <img
                v-for="(cutout, index) in cutouts"
                :key="cutout.id"
                :src="cutout.image"
                class="pile-cutout"
                draggable="true"
                @dragstart="dragStart(index, $event)"
            />
        </div>
    </div>
</template>

<script setup>
import { ref, reactive } from "vue";

// Static puzzle cutouts (to drag)
const cutouts = ref([
    { id: 1, image: "/assets/cutout1.jpg" },
    { id: 2, image: "/assets/cutout2.jpg" },
    { id: 3, image: "/assets/cutout3.jpg" },
]);

// Gaps on the puzzle board (drop zones) - Initially estimated positions
const gaps = reactive([
    { id: 1, filled: null, style: "top: 40px; left: 100px" },
    { id: 2, filled: null, style: "top: 120px; left: 210px" },
    { id: 3, filled: null, style: "top: 220px; left: 150px" },
]);

// Debugging helper: cutouts for manual positioning
const debugCutouts = reactive([
    { id: 1, image: "/assets/cutout1.jpg", top: 40, left: 100 },
    { id: 2, image: "/assets/cutout2.jpg", top: 120, left: 210 },
    { id: 3, image: "/assets/cutout3.jpg", top: 220, left: 150 },
]);

// Handle drag start (real game pieces)
const dragStart = (index, event) => {
    event.dataTransfer.setData("cutoutIndex", index);
};

// Drop cutout into the correct gap
const dropPiece = (gapIndex, event) => {
    const cutoutIndex = parseInt(event.dataTransfer.getData("cutoutIndex"), 10);
    const cutout = cutouts.value[cutoutIndex];

    if (!gaps[gapIndex].filled) {
        gaps[gapIndex].filled = cutout;
        cutouts.value.splice(cutoutIndex, 1);
    }
};

// Debugging: Manually position cutouts by dragging them
const startDrag = (index, event) => {
    const cutout = debugCutouts[index];
    let startX = event.clientX;
    let startY = event.clientY;

    const moveHandler = (e) => {
        const dx = e.clientX - startX;
        const dy = e.clientY - startY;
        cutout.top += dy;
        cutout.left += dx;
        startX = e.clientX;
        startY = e.clientY;
    };

    const stopHandler = () => {
        console.log(
            `Cutout ${cutout.id} positioned at: top: ${cutout.top}px; left: ${cutout.left}px`,
        );
        document.removeEventListener("mousemove", moveHandler);
        document.removeEventListener("mouseup", stopHandler);
    };

    document.addEventListener("mousemove", moveHandler);
    document.addEventListener("mouseup", stopHandler);
};
</script>

<style scoped>
.puzzle-wrapper {
    display: flex;
    flex-direction: column;
    align-items: center;
}

.puzzle-board {
    position: relative;
    width: 500px;
    height: 400px;
    margin-bottom: 20px;
}

.puzzle-bg {
    width: 100%;
    height: auto;
    display: block;
    border: 2px solid #ccc;
    border-radius: 10px;
}

/* Gaps (drop zones) on the image */
.puzzle-gap {
    position: absolute;
    width: 60px;
    height: 60px;
    border: 2px dashed rgba(0, 0, 0, 0.3);
    border-radius: 8px;
    background-color: rgba(255, 255, 255, 0.5);
    display: flex;
    align-items: center;
    justify-content: center;
}

.puzzle-cutout {
    width: 100%;
    height: auto;
}

/* Draggable pile of cutouts */
.cutouts-pile {
    display: flex;
    gap: 10px;
    flex-wrap: wrap;
    justify-content: center;
}

.pile-cutout {
    width: 60px;
    height: 60px;
    cursor: grab;
    border: 1px solid #ccc;
    border-radius: 8px;
    background-color: #fff;
}

/* Debug Mode Styling */
.debug-mode {
    margin-top: 20px;
    background: #f8f8f8;
    padding: 10px;
    border: 1px solid #ddd;
}

.debug-cutout {
    position: absolute;
    width: 60px;
    height: 60px;
    cursor: grab;
    border: 2px solid red;
    border-radius: 8px;
    background-color: rgba(255, 0, 0, 0.3);
}
</style>

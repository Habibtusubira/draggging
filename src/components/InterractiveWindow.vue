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
                :style="{
                    top: gap.top + 'px',
                    left: gap.left + 'px',
                    width: gap.width + 'px',
                    height: gap.height + 'px',
                }"
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

        <!-- Pile of cutouts to pick from -->
        <div class="cutouts-pile">
            <img
                v-for="(cutout, index) in cutouts"
                :key="cutout.id"
                :src="cutout.image"
                class="pile-cutout"
                :style="{
                    width: cutout.width + 'px',
                    height: cutout.height + 'px',
                }"
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
    { id: 1, image: "/assets/cutout1.jpg", width: 539, height: 359 },
    { id: 2, image: "/assets/cutout2.jpg", width: 665, height: 444 },
    { id: 3, image: "/assets/cutout3.jpg", width: 356, height: 238 },
]);

// Gaps on the puzzle board (drop zones) - Set same width & height as cutouts
const gaps = reactive([
    { id: 1, filled: null, top: 0, left: 0, width: 539, height: 359 },
    { id: 2, filled: null, top: 410, left: 730, width: 665, height: 430 },
    { id: 3, filled: null, top: 590, left: 54, width: 356, height: 238 },
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
</script>

<style scoped>
.puzzle-wrapper {
    display: flex;
    flex-direction: column;
    align-items: center;
}

.puzzle-board {
    position: relative;
    width: 1394px;
    height: 850px;
    margin-bottom: 20px;
}

.puzzle-bg {
    width: 100%;
    height: auto;
    display: block;
    border: 2px solid #ccc;
    border-radius: 10px;
}

/* Gaps (drop zones) now match the cutout sizes */
.puzzle-gap {
    position: absolute;
    border: 2px dashed rgba(0, 0, 0, 0.3);
    border-radius: 8px;
    background-color: rgba(255, 255, 255, 0.5);
    display: flex;
    align-items: center;
    justify-content: center;
}

/* Cutouts fit exactly into their drop zones */
.puzzle-cutout {
    width: 100%;
    height: 100%;
}

/* Draggable pile of cutouts */
.cutouts-pile {
    display: flex;
    gap: 10px;
    flex-wrap: wrap;
    justify-content: center;
}

.pile-cutout {
    cursor: grab;
    border: 1px solid #ccc;
    border-radius: 8px;
    background-color: #fff;
}
</style>

<script lang="ts" setup>
import VueDragReSizeRotate from "@gausszhou/vue3-drag-resize-rotate";
import "@gausszhou/vue3-drag-resize-rotate/lib/bundle.esm.css";
import { computed } from "vue";
import { ElementType } from '../enums/ElementType'

const props = defineProps({
    elementType: String,
    rotatable: Boolean,
    w: {
        type: [Number, String],
        default: 'auto'
    },
    h: {
        type: [Number, String],
        default: 'auto'
    },
    x: {
        type: [Number],
        default: 0
    },
    y: {
        type: [Number],
        default: 0
    },
})

const _h = computed(() => {
    if (props.elementType == ElementType.RECTANGLE) {
        return 50
    }
    return props.h
})

const _w = computed(() => {
    if (props.elementType == ElementType.RECTANGLE) {
        return 50
    }
    return props.w
})
</script>

<template>
    <VueDragReSizeRotate :rotatable="props.rotatable" :parent="true" :w="_w" :h="_h" :x="props.x" :y="props.y" :min-height="10" :min-width="10">
        <div v-if="props.elementType == ElementType.TEXT" contenteditable="true" class="outline-none font-bold text-lg text-red-500">xxx</div>
        <div v-if="props.elementType == ElementType.RECTANGLE" class="border h-full w-full"></div>
    </VueDragReSizeRotate>
</template>
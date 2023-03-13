<script lang="ts" setup>
import { ref } from 'vue';
import Element from './Element.vue'
import { onClickOutside } from '@vueuse/core'
import { ElementType } from '../enums/ElementType';
import { isEmpty } from 'lodash'

type element = {
    elementType: ElementType,
    rotatable: boolean,
    w?: number,
    h?: number | string,
    x: number,
    y: number,
}

/* Refs */
const edit = ref(null);
const elements = ref([] as element[]);
const activeTool = ref(ElementType.NO_TOOL)

/* Events */
onClickOutside(edit, (e) => {
    resetTool()
})

/* Methods */
const activateTool = (tool: ElementType) => {
    activeTool.value = tool
}

const resetTool = () => {
    activeTool.value = ElementType.NO_TOOL
}

const clickEvent = (e: MouseEvent) => {
    if (! isEmpty(activeTool)
        && activeTool.value != ElementType.NO_TOOL) {
        const clickPositionX = e.layerX
        const clickPositionY = e.layerY
        elements.value.push({
            elementType: activeTool.value,
            rotatable: true,
            w: 50,
            h: 'auto',
            x: clickPositionX,
            y: clickPositionY
        })
        resetTool()
    }
}

</script>

<template>
    <div class="h-full w-full flex flex-col items-center justify-center">
        <div ref="edit" class="relative w-full max-w-lg border h-96 shadow-md bg-center" @click="(e) => clickEvent(e)" style="background-image: url('/bg.jpeg');">
            <Element v-for="element in elements" v-bind="element" />
        </div>
        <div class="mt-5">
            <ul class="flex border rounded-lg">
                <li>
                    <button class="p-2" :class="{ 'bg-gray-200': activeTool == ElementType.TEXT }" @click="activateTool(ElementType.TEXT)">
                        <svg xmlns="http://www.w3.org/2000/svg" class="icon icon-tabler icon-tabler-typography w-5 h-5" width="24" height="24" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" fill="none" stroke-linecap="round" stroke-linejoin="round">
                            <path stroke="none" d="M0 0h24v24H0z" fill="none"></path>
                            <path d="M4 20l3 0"></path>
                            <path d="M14 20l7 0"></path>
                            <path d="M6.9 15l6.9 0"></path>
                            <path d="M10.2 6.3l5.8 13.7"></path>
                            <path d="M5 20l6 -16l2 0l7 16"></path>
                        </svg>
                    </button>
                </li>
                <li>
                    <button class="p-2 border-l" :class="{ 'bg-gray-200': activeTool == ElementType.RECTANGLE }" @click="activateTool(ElementType.RECTANGLE)">
                        <svg xmlns="http://www.w3.org/2000/svg" class="icon icon-tabler icon-tabler-square w-5 h-5" width="24"
                            height="24" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" fill="none"
                            stroke-linecap="round" stroke-linejoin="round">
                            <path stroke="none" d="M0 0h24v24H0z" fill="none"></path>
                            <path d="M3 3m0 2a2 2 0 0 1 2 -2h14a2 2 0 0 1 2 2v14a2 2 0 0 1 -2 2h-14a2 2 0 0 1 -2 -2z"></path>
                        </svg>
                    </button>
                </li>
            </ul>
        </div>
    </div>
</template>
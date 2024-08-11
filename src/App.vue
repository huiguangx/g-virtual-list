<template>
  <h1>virtual list</h1>
  <div ref="container" class="container" :style="styleObject">
    <div class="scroll-blank"></div>
    <div class="scroll">
      <div v-for="item in activeList" :key="item" class="scroll-item">
        {{ item }}
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { computed, onMounted, ref } from "vue";

interface StyleObject {
  [key: string]: string | number;
}

const container = ref<HTMLElement | null>(null);
const start = ref<number>(0);
const marginTop = ref<number>(0);
const itemHeight: number = 20;
const num: number = 20;
const list: number[] = createList();

const containerHeight = computed(() => num * itemHeight);
const height = computed(() => list.length * itemHeight);
const activeList = computed(() => list.slice(start.value, start.value + num));
const styleObject = computed<StyleObject>(() => ({
  "--containerHeight": `${containerHeight.value}px`,
  "--height": `${height.value}px`,
  "--itemHeight": `${itemHeight}px`,
  "--marginTop": `${marginTop.value}px`,
}));

function createList(): number[] {
  return Array.from({ length: 1000 }, (_, i) => i);
}

onMounted(() => {
  if (container.value) {
    container.value.addEventListener("scroll", onScroll);
  }
});

function onScroll(event: Event): void {
  const { scrollTop } = event.target as HTMLElement;
  start.value = Math.floor(scrollTop / itemHeight);
  marginTop.value = scrollTop;
}
</script>

<style scoped>
.container {
  width: 30vw;
  display: flex;
  border: 1px solid #e1e1e1;
  overflow-y: auto;
  height: var(--containerHeight);
  border-radius: 8px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  background-color: #ffffff;
}
.scroll-blank {
  height: var(--height);
}
.scroll {
  margin-top: var(--marginTop);
}
.scroll-item {
  height: var(--itemHeight);
  /* background-color: pink; */
}
</style>

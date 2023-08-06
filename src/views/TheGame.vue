<script setup>
import { ref, reactive, watch } from "vue";

const sizeX = ref(5);
const sizeY = ref(5);

const array = reactive({ value: Array(sizeX.value * sizeY.value) });

watch([sizeX, sizeY], () => {
  array.value = Array(sizeX.value * sizeY.value).fill(false);
});

const toggleSquare = (index) => {
  array.value[index] = !array.value[index];
};
</script>

<template>
  <v-sheet width="300" class="mx-auto mt-10">
    <v-form @submit.prevent>
      <v-text-field v-model="sizeX" label="sizeX"></v-text-field>

      <v-text-field v-model="sizeY" label="sizeY"></v-text-field>
    </v-form>
  </v-sheet>

  <v-card
    class="mx-auto pa-12 pb-8 mt-10"
    elevation="8"
    max-width="1200"
    rounded="lg"
  >
    <div style="overflow-x: auto">
      <div
        class="mt-10"
        :style="{
          display: 'grid',
          placeItems: 'center',
          gap: '5px',
          gridTemplateColumns: `repeat(${sizeX}, 1fr)`,
          background: '#332f2f',
          padding: '5px',
        }"
      >
        <div
          @mouseover="toggleSquare(index)"
          :style="{
            display: 'flex',
            height: '36px',
            width: '36px',
            backgroundColor: square ? 'blue' : 'white',
            margin: '16px',
          }"
          v-for="(square, index) in array.value"
          :key="index"
        ></div>
      </div>
    </div>
  </v-card>
</template>

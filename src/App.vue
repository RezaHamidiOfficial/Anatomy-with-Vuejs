<script setup lang="ts">
import { Ref, ref } from "vue";
import GRAnatomi from "@/components/GRAnatomi.vue";
import Biography from "@/types/Biography";

const selectDropped: Ref<Biography[]> = ref([]);
</script>

<template>
  <div class="d-flex justify-content-center w-100">
    <GRAnatomi
      :set-dropped-elements="selectDropped"
      image-url="/body.png"
      v-model="selectDropped"
    />
  </div>
</template>

<style scoped></style>

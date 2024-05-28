<script setup lang="ts">
// Pakages
import { ref, Ref, PropType } from "vue";

// Component
import GRButton from "@/components/GRButton.vue";
import GRModal from "@/components/GRModal.vue";

// TypeScript
import Biography from "@/types/Biography";

const props = defineProps({
  imageUrl: {
    type: String as PropType<string>,
    required: true,
  },
  id: {
    type: [Number, String] as PropType<number | string | null>,
    default: null,
  },
  colorsRange: {
    type: String as PropType<"rgba(69, 242, 249, 1)">,
  },
  setDroppedElements: {
    type: Array as PropType<Biography[]>,
    required: true,
  },
});

function sizeToValue(size: string) {
  switch (size) {
    case "small":
      return 10;
  }
}

const nextDroppedElementId: Ref<number> = ref(1);
const changeLocation: Ref<boolean> = ref(true);

const CircleSize: Ref<string> = ref("small");
const saveIdDelete = ref(0);
const imageHeight: Ref<number> = ref(616);
const imageWidth: Ref<number> = ref(350);

const deleteModelModal = ref({
  visible: false,
});

const openModalDelete = async (index) => {
  deleteModelModal.value = {
    visible: true,
  };
  saveIdDelete.value = index;
};

async function deleteModel() {
  props.setDroppedElements.splice(saveIdDelete.value, 1);
  changeLocation.value = !changeLocation.value;
  deleteModelModal.value = {
    visible: false,
  };
}

async function onDrop(event) {
  changeLocation.value = !changeLocation.value;

  const sizeValue = sizeToValue(CircleSize.value);
  const droppedElement = ref({
    id: nextDroppedElementId.value,
    size: CircleSize.value,
    locationX: event.offsetX - sizeValue,
    locationY: event.offsetY - sizeValue,
  });
  nextDroppedElementId.value++;
  props.setDroppedElements.push(droppedElement.value);
}
</script>

<template>
  <div>
    <div class="mt-4">
      <div class="drag-drop-container">
        <div
          class="drop-box mt-4"
          :style="{
            background: `url(` + props.imageUrl + `)`,
            height: imageHeight + 'px',
            width: imageWidth + 'px !important',
          }"
          @click="onDrop"
          id="imageSize"
        >
          <div
            @click.stop
            v-for="(droppedElement, index) in setDroppedElements"
            :key="index"
            :style="{
              left: droppedElement.locationX + 'px',
              top: droppedElement.locationY + 'px',
            }"
            @click="openModalDelete(index)"
            class="dropped-box circle"
            :class="droppedElement.size"
            draggable="false"
          ></div>
        </div>

        <GRModal blur v-model="deleteModelModal.visible" :prevent-close="true">
          <div class="p-1 rounded-3 text-align">
            <div class="list-title">
              <span class="fw-bold text-large text-color text-align">
                Are you sure you want to delete this item?
              </span>
            </div>
            <div
              class="d-flex justify-content-around align-items-center gap-2 mt-5"
            >
              <GRButton
                title="cancel"
                color="gray"
                @click="deleteModelModal.visible = false"
              />
              <GRButton title="delete" color="gradient" @click="deleteModel" />
            </div>
          </div>
        </GRModal>
      </div>
    </div>
  </div>
</template>

<style scoped>
.drag-drop-container {
  justify-content: space-between;
}

.drag-box {
  position: absolute;
  z-index: 1000;
  top: 0;
  left: 0;
  height: 200px;
  text-align: center;
  line-height: 100px;
  padding: 51px;
}

.drop-box {
  background-size: contain !important;
  background-repeat: no-repeat !important;
  background-position: center !important;
  width: 100%;
  overflow: hidden;
  position: relative;
  text-align: center;
  line-height: 100px;
  position: relative;
}

.box {
  width: 110px;
  height: 100px;
  border-radius: 10px;
  background-color: #636363;
  color: white;
}

.dropped-box {
  position: absolute;

  border-radius: 50%;
  background-color: rgba(69, 242, 249, 0.7);
  justify-content: center;
  align-items: center;
  cursor: pointer;
}
.box-btns {
  border: none;
  border-radius: 10px;
  margin: 2px;
  padding: 0 !important;
}
.small {
  width: 18px;
  height: 18px;
}
</style>

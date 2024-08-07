# Anatomy-with-Vuejs

<p float="left">
    <img src="https://github.com/RezaHamidi0/Anatomy-with-Vuejs/assets/103819181/eabe9da1-b42f-4407-8fdc-95b22b39b9f3" width="250" />
    <img src="https://github.com/RezaHamidi0/Anatomy-with-Vuejs/assets/103819181/d5202974-b041-410f-97bd-07e5f3eae247" width="250" />
    <img src="https://github.com/user-attachments/assets/6dc1aeef-919a-43d0-bf0c-9bffe7be2f35" width="250" />
</p>

<p float="left">This project is a Vue.js component that allows users to select and highlight pain points on a body image. It can be used for educational purposes or as part of a medical application.</p>

<br />

## Features

- **Interactive Pain Selection**: Users can click on the body image to indicate where they are experiencing pain. The selected areas are highlighted with a specific color.
  
- **Modal for Deletion**: Users can delete a marked pain point by clicking on it and confirming the deletion in a modal.
  
- **Dynamic Sizes**: Pain points can be of different sizes to indicate the intensity or area of pain.

<br />

## Installation

1. Install dependencies:

    ```sh
    npm install
    ```

2. Run the project:

    ```sh
    npm run serve
    ```
<br />

## Usage

To use this component in your project, follow these steps:

1. Import the component in your Vue.js file:

    ```vue
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
    ```

2. Customize the component props as needed:
    - `imageUrl`: The URL of the body image.
    - `setDroppedElements`: An array to store the selected pain points.
    - `id`: Optional identifier for the component.
    - `colorsRange`: Color used to highlight the pain points.


<br />

## Component Code

### GRAnatomi.vue
```vue
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
```

<br />

### Biography.ts
```typescript
export default interface boxElement {
  id: number|string;
  size: string;
  locationX: number,
  locationY: number,
}
```

<br />

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for more details.

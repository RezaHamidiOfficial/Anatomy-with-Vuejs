<template>
  <transition name="modal">
    <div
      v-if="modelValue"
      @click="preventClose ? shakeModal() : closeModal()"
      :style="{ zIndex: 999990 + order }"
      class="gr-modal-wrapper p-3"
      :class="{ 'blur-overlay': blur }"
    >
      <div
        class="gr-modal background-color"
        :class="[noPadding ? 'p-0' : 'p-3', { 'full-screen': fullScreen }]"
        ref="GRModal"
        @click.stop
      >
        <GRButton
          v-if="!preventClose"
          color="transparent"
          class="closeBtn"
          fixed-width-height="34"
          @click="closeModal"
          style="border-radius: 50px !important; border: 2px solid #fff"
        >
          <template #content>
            <CloseIcon width="16" fill="#fff" />
          </template>
        </GRButton>
        <slot />
      </div>
    </div>
  </transition>
</template>

<script setup lang="ts">
import { PropType, Ref, ref } from "vue";
import GRButton from "@/components/GRButton.vue";
import CloseIcon from "@/assets/icons/CloseIcon.vue";

defineProps({
  modelValue: {
    type: Boolean as PropType<boolean>,
  },
  blur: {
    type: Boolean as PropType<boolean>,
    default: false,
  },
  preventClose: {
    type: Boolean as PropType<boolean>,
    default: false,
  },
  noPadding: {
    type: Boolean as PropType<boolean>,
    default: false,
  },
  fullScreen: {
    type: Boolean as PropType<boolean>,
    default: false,
  },
  order: {
    type: Number as PropType<number>,
    default: 0,
  },
});

const emit = defineEmits(["onClose", "update:modelValue"]);

const GRModal: Ref<HTMLDivElement> = ref(null);

function shakeModal() {
  GRModal.value.classList.add("shake");
  setTimeout(() => {
    GRModal.value.classList.remove("shake");
  }, 500);
}

function closeModal() {
  emit("update:modelValue", false);
}
</script>

<style scoped lang="scss">
.gr-modal-wrapper {
  width: 100%;
  height: 100vh;
  background: rgba(0, 0, 0, 0.2);
  position: fixed;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  display: flex;
  justify-content: center;
  align-items: center;

  @media screen and (max-width: 450px) {
    .gr-modal {
      min-width: 300px !important;
    }
  }

  .gr-modal {
    border-radius: 20px;
    box-shadow: 0 5px 30px 0 rgba(0, 0, 0, 0.1);
    background-color: rgb(255, 255, 255);
    position: relative;
    min-width: 420px;
    max-width: 95vw;
    max-height: 95vh;

    &.full-screen {
      width: 100%;
      height: 100%;
    }

    &.shake {
      animation: shake 0.3s;
    }

    .closeBtn {
      position: absolute;
      right: 15px;
      top: -40px;
      border-radius: 50px;
      box-shadow: 0 5px 30px 0 rgba(0, 0, 0, 0.09);

      svg {
        fill: rgb(255, 255, 255);
      }
    }
  }
}

[data-theme="darkMode"] {
  .closeBtn {
    svg {
      fill: rgb(255, 255, 255) !important;
    }
  }
}

.modal-enter-active {
  transition: all 300ms ease;
}

.modal-enter-from,
.modal-leave-to {
  opacity: 0;

  .gr-modal {
    transform: scale(0.5);
  }
}

.modal-enter-to {
  opacity: 1;

  .gr-modal {
    transform: scale(1);
  }
}

@keyframes shake {
  0% {
    transform: translate(1px, 1px) rotate(0deg);
  }
  10% {
    transform: translate(-1px, -2px) rotate(-1deg);
  }
  20% {
    transform: translate(-3px, 0px) rotate(1deg);
  }
  30% {
    transform: translate(3px, 2px) rotate(0deg);
  }
  40% {
    transform: translate(1px, -1px) rotate(1deg);
  }
  50% {
    transform: translate(-1px, 2px) rotate(-1deg);
  }
  60% {
    transform: translate(-3px, 1px) rotate(0deg);
  }
  70% {
    transform: translate(3px, 1px) rotate(-1deg);
  }
  80% {
    transform: translate(-1px, -1px) rotate(1deg);
  }
  90% {
    transform: translate(1px, 2px) rotate(0deg);
  }
  100% {
    transform: translate(1px, -2px) rotate(-1deg);
  }
}
</style>

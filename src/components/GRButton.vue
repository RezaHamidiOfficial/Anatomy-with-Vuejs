<template>
  <div
    class="gr-button"
    :style="fixedWidthHeight !== null && fixedWidthHeightStyles"
  >
    <button
      :disabled="disabled || loading"
      :class="[
        getClasses,
        loading && 'opacity-10',
        disabled && 'opacity-50',
        { active: active },
        fixedWidthHeight ? 'h-100' : !customPadding && 'gr-button-padding',
      ]"
      class="IRANYekan w-100"
      :style="customPadding && 'padding: ' + customPadding"
      tabindex="0"
      @click="$emit('click')"
    >
      <slot name="content">{{ title }}</slot>
    </button>

    <div
      class="gr-tooltip text-xSmall IRANYekan no-pointer-event"
      style="color: #fff"
      v-if="tooltip"
    >
      {{ tooltip }}
    </div>

    <div class="custom-loader center" v-if="loading" />
  </div>
</template>

<script setup lang="ts">
import { computed, ComputedRef, PropType } from "vue";

const props = defineProps({
  title: {
    type: String as PropType<string>,
  },
  tooltip: {
    type: String as PropType<string>,
    default: null,
  },
  theme: {
    type: String as PropType<"modern" | "flat">,
    default: "modern",
    validator: (prop: string) => ["modern", "flat"].includes(prop),
  },
  color: {
    type: String as PropType<
      | "linearOrange"
      | "secondary"
      | "primary"
      | "blueLight"
      | "warn"
      | "bluePurple"
      | "gray"
      | "danger"
      | "gradient"
      | "success"
      | "dark"
      | "white"
      | "transparent"
    >,
    default: "secondary",
    validator: (prop: string) =>
      [
        "linearOrange",
        "secondary",
        "primary",
        "blueLight",
        "warn",
        "bluePurple",
        "gray",
        "danger",
        "gradient",
        "success",
        "dark",
        "white",
        "transparent",
      ].includes(prop),
  },
  size: {
    type: String as PropType<"mini" | "small" | "medium" | "large">,
    default: "small",
    validator: (prop: string) =>
      ["mini", "small", "medium", "large"].includes(prop),
  },
  btnBlock: {
    type: Boolean as PropType<boolean>,
    default: false,
  },
  radius: {
    type: String as PropType<"round" | "circle" | "square">,
    default: "round",
    validator: (prop: string) => ["round", "circle", "square"].includes(prop),
  },
  bold: {
    type: Boolean as PropType<boolean>,
    default: true,
  },
  loading: {
    type: Boolean as PropType<boolean>,
    default: false,
  },
  disabled: {
    type: Boolean as PropType<boolean>,
    default: false,
  },
  fixedWidthHeight: {
    type: [String, null] as PropType<string | null>,
    default: null,
  },
  active: {
    type: Boolean as PropType<boolean>,
    default: false,
  },
  customPadding: {
    type: String as PropType<string>,
    default: null,
  },
});

defineEmits(["click"]);

const fixedWidthHeightStyles =
  "width:" +
  props.fixedWidthHeight +
  "px;" +
  "height:" +
  props.fixedWidthHeight +
  "px;" +
  "padding: 0";

const getClasses: ComputedRef<string> = computed(() => {
  let classes = `${props.theme} ${props.color} ${props.size} ${props.radius}`;
  !!props.bold && (classes += " fw-bold");
  !!props.btnBlock && (classes += " w-100");
  return classes;
});
</script>
<style lang="scss">
.gr-button {
  position: relative;

  button {
    display: flex;
    justify-content: center;
    align-items: center;
    text-align: center;
  }

  .modern,
  .flat {
    border: none;
    outline: none;
  }

  .gr-button-padding {
    padding: 11px 26px;
  }

  // radius
  .circle {
    border-radius: 999px;
  }

  .square {
    border-radius: 0;
  }

  .round {
    border-radius: 8px;
  }

  // tooltip
  .gr-tooltip {
    visibility: hidden;
    opacity: 0;
  }

  &:hover {
    .gr-tooltip {
      visibility: visible;
      opacity: 10;
      transform: translate(-20%, -75px);
    }
  }

  // color
  .flat {
    &:focus,
    &.active {
      svg {
        fill: rgba(var(--white), 1) !important;
        stroke: rgba(var(--white), 1) !important;
      }
    }

    &.linearOrange {
      background-image: linear-gradient(to bottom right, #ffb703, #fb8500);
      //color: rgba(var(--secondary), 1);
      //background: rgba(var(--secondary), .1);

      &:hover {
        //background: rgba(var(--secondary), .2);
        background-image: linear-gradient(to bottom right, #ffb703, #fb8500);
      }

      &:focus {
        background-image: linear-gradient(to bottom right, #ffb703, #fb8500);
        //background: rgba(var(--secondary), 1);
        //color: rgba(var(--white), 1);
      }
    }

    &.secondary {
      color: rgba(var(--secondary), 1);
      background: rgba(var(--secondary), 0.1);

      &:hover {
        background: rgba(var(--secondary), 0.2);
      }

      &:focus {
        background: rgba(var(--secondary), 1);
        color: rgba(var(--white), 1);
      }
    }

    &.primary {
      color: rgba(var(--primary), 1);
      background: rgba(var(--primary), 0.1);

      &:hover {
        background: rgba(var(--primary), 0.2);
      }

      &:focus {
        background: rgba(var(--primary), 1);
        color: rgba(var(--white), 1);
      }
    }

    &.blueLight {
      color: rgba(var(--text-color), 1);
      background: rgba(var(--blueLight), 0.1);

      &:hover {
        background: rgba(var(--blueLight), 0.2);
      }

      &:focus {
        background: rgba(var(--blueLight), 1);
        color: rgba(var(--white), 1);
      }
    }

    &.warn {
      color: rgba(var(--warn), 1);
      background: rgba(var(--warn), 0.2);

      &:hover {
        background: rgba(var(--warn), 0.3);
      }

      &:focus {
        background: rgba(var(--warn), 1);
        color: rgba(var(--white), 1);
      }
    }

    &.bluePurple {
      color: rgba(var(--background-color-blue-purple-light), 1);
      background: rgba(var(--background-color-blue-purple-light), 0.2);

      &:hover {
        background: rgba(var(--background-color-blue-purple-light), 0.3);
      }

      &:focus {
        background: rgba(var(--background-color-blue-purple-light), 1);
        color: rgba(var(--white), 1);
      }
    }

    &.danger {
      color: rgba(var(--gray), 1);
      background: rgba(var(--gray), 0.1);

      &:hover {
        background: rgba(var(--gray), 0.2);
      }

      &:focus {
        background: rgba(var(--gray), 1);
        color: rgba(var(--white), 1);
      }
    }

    &.danger {
      color: rgba(var(--danger), 1);
      background: rgba(var(--danger), 0.1);

      &:hover {
        background: rgba(var(--danger), 0.2);
      }

      &:focus {
        background: rgba(var(--danger), 1);
        color: rgba(var(--white), 1);
      }
    }

    &.gradient {
      color: rgb(85, 148, 251);
      background: linear-gradient(
        270deg,
        rgb(67, 247, 248) 6.44%,
        rgb(85, 148, 251) 100%
      );

      &:hover {
        background: rgba(85, 148, 251, 0.2);
      }

      &:focus {
        background: rgba(85, 148, 251, 1);
        color: rgba(var(--white), 1);
      }
    }

    &.success {
      color: rgba(var(--success), 1);
      background: rgba(var(--success), 0.1);

      &:hover {
        background: rgba(var(--success), 0.2);
      }

      &:focus {
        background: rgba(var(--success), 1);
        color: rgba(var(--white), 1);
      }
    }

    &.dark {
      color: rgba(var(--text-color), 1);
      background: rgba(var(--black), 0.1);

      svg path {
        fill: rgba(var(--text-color), 1) !important;
      }

      &:hover {
        background: rgba(var(--black), 0.2);
      }

      &:focus {
        background: rgba(var(--black), 1);
        color: rgba(var(--white), 1);
      }
    }
  }

  .modern,
  .flat.active {
    &.primary {
      background: rgba(var(--primary), 1);

      &:hover {
        box-shadow: 0 4px 9px 2px rgba(var(--primary), 0.3);
      }
    }

    &.blueLight {
      background: rgba(var(--blueLight), 1);

      &:hover {
        box-shadow: 0 4px 9px 2px rgba(var(--blueLight), 0.3);
      }
    }

    &.warn {
      background: rgba(var(--warn), 1);

      &:hover {
        box-shadow: 0 4px 9px 2px rgba(var(--warn), 0.3);
      }
    }

    &.bluePurple {
      background: rgba(var(--background-color-blue-purple-light), 1);

      &:hover {
        box-shadow: 0 4px 9px 2px
          rgba(var(--background-color-blue-purple-light), 0.3);
      }
    }

    &.gray {
      background: rgba(var(--gray), 1);

      &:hover {
        box-shadow: 0 4px 9px 2px rgba(var(--gray), 0.3);
      }
    }

    &.danger {
      background: rgba(var(--danger), 1);

      &:hover {
        box-shadow: 0 4px 9px 2px rgba(var(--danger), 0.3);
      }
    }

    &.gradient {
      background: linear-gradient(
        270deg,
        #43f7f8 6.44%,
        rgb(85, 148, 251) 100%
      );
      box-shadow: 0px 10px 20px 0px rgba(20, 42, 91, 0.2);

      &:hover {
        box-shadow: 5px 5px 25px 2px rgba(0, 0, 0, 0.05);
      }
    }

    &.success {
      background: rgba(var(--success), 1);

      &:hover {
        box-shadow: 0 4px 9px 2px rgba(var(--success), 0.3);
      }
    }

    &.dark {
      background: rgba(var(--black), 1);

      * {
        color: rgb(var(--white));
      }

      &:hover {
        box-shadow: 0 4px 9px 2px rgba(var(--black), 0.3);
      }
    }

    &.white {
      background: rgba(var(--white), 1);
      color: rgba(var(--text-color), 1);

      &:hover {
        box-shadow: 0 4px 9px 2px rgba(var(--black), 0.05);
      }
    }

    &.secondary {
      background: rgba(var(--secondary), 1);

      &:hover {
        box-shadow: 0 4px 9px 2px rgba(var(--secondary), 0.3);
      }
    }

    &.primary,
    &.warn,
    &.bluePurple,
    &.gray,
    &.danger,
    &.gradient,
    &.success,
    &.dark,
    &.secondary {
      color: rgba(var(--white), 1);
    }
    &.blueLight {
      color: rgba(var(--text-color), 1);
    }
  }

  .transparent {
    background: transparent;
    color: rgb(var(--white));
  }

  .mini {
    font-size: 10px;
  }

  .small {
    font-size: 12px;
  }

  .medium {
    font-size: 14px;
  }

  .large {
    font-size: 16px;
  }
}
</style>

<template>
  <div :style="finalStyle"></div>
</template>
<script>
import { inject, computed } from "vue";
const inBrowser = typeof window !== "undefined";
export default {
  name: "VueProgress",
  serverCacheKey: () => "Progress",
  setup() {
    const progress = inBrowser
      ? inject("RADON_LOADING_BAR")
      : {
          percent: 0,
          options: {
            canSuccess: true,
            show: false,
            color: "rgb(19, 91, 55)",
            failedColor: "red",
            thickness: "2px",
            transition: {
              speed: "0.2s",
              opacity: "0.6s",
              termination: 300,
            },
            autoRevert: true,
            inverse: false,
          },
        };

    const finalStyle = computed(() => {
      const options = progress.options;
      const isShow = !!options.show;
      const style = {
        "z-index": 999999,
        "background-color": options.canSuccess
          ? options.color
          : options.failedColor,
        opacity: options.show ? 1 : 0,
        position: options.position,
      };
      if (!options.inverse) {
        style.left = "0px";
      } else {
        style.right = "0px";
      }
      style.width = progress.percent + "%";
      style.height = options.thickness;
      style.transition =
        (isShow ? "width " + options.transition.speed + ", " : "") +
        "opacity " +
        options.transition.opacity;
      return style;
    });

    return {
      progress,
      finalStyle,
    };
  },
};
</script>

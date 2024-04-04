<template>
  <q-page-sticky class="stickyClass" position="top-right" :offset="[10, 10]">
    <q-card class="my-card" flat bordered style="width: 400px;height:50vh;overflow-y:auto">
      <q-card style="padding: 10px; margin-top: 32px">
        <q-input
          v-for="(item, index) in Object.keys(selected)"
          :key="index"
          :label="item"
          v-model="object[item]"
        />
      </q-card>
      <q-carousel-control
        position="top-left"
        class="text-white rounded-borders"
      >
        <q-btn
          class="absolute shadow-2 closeClass"
          round
          color="white"
          text-color="black"
          icon="close"
          size="sm"
          @click="closeCard"
        />
      </q-carousel-control>
    </q-card>
  </q-page-sticky>
</template>

<script>
import {
  ref,
  unref,
  onMounted,
  defineComponent,
  getCurrentInstance,
  watchEffect,
} from "vue";
import { $bus } from "boot/bus.js";
import { updateFeature } from "src/api/feature";

import DetailTable from "src/components/floatDetail/detailTable.vue";
import { LAYER_TYPE, FEATURE_TYPE } from "src/constants/enum";
import { useMapStore } from "stores/map";
import { updateXML } from "src/utils/transactionXML";

export default defineComponent({
  name: "FloatDetail",
  components: {
    // "detail-table": DetailTable,
  },
  props: {
    value: String,
    selected: Object,
  },
  setup(props, { emit }) {
    const object = ref(props.selected);

    // Watch for changes in the prop 'selected'
    watchEffect(() => {
      object.value = props.selected;
    });

    const closeCard = () => {
      $bus.emit("close-float-detail", true);
      emit("update:model-value", false);
    };
    return {
      object,
      closeCard,
    };
  },
});
</script>
<style lang="scss" scoped>
html,
body {
  width: 100%;
  height: 100%;
}

.stickyClass {
  z-index: 1;
}

.panelClass {
  max-height: 230px;
  max-width: 400px;
}

.closeClass {
  cursor: pointer;
}

::-webkit-scrollbar {
  height: 12px;
  width: 14px;
  background: transparent;
  z-index: 12;
  overflow: visible;
  cursor: auto !important;
}

::-webkit-scrollbar-thumb {
  width: 10px;
  background-color: $secondary;
  border-radius: 10px;
  z-index: 12;
  border: 4px solid rgba(0, 0, 0, 0);
  background-clip: padding-box;
  transition: background-color 0.32s ease-in-out;
  margin: 4px;
  min-height: 32px;
  min-width: 32px;
  cursor: auto !important;
}

::-webkit-scrollbar-thumb:hover {
  background: $secondary;
  cursor: auto !important;
}

.my-card {
  .q-carousel__navigation-inner {
    display: none !important;
  }
}
</style>

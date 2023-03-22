<template>
  <q-card id="interaction-menu">
    <q-item class="general-content flex column items-center">
      <q-item-section class="avatar-section flex items-center">
        <q-avatar class="avatar">
          <img :src="`src/assets/${data.avatar}`" alt="Avatar" />
        </q-avatar>
      </q-item-section>

      <q-item-section>
        <div class="text-h6 text-white text-center">{{ data.title }}</div>
        <div class="text-subtitle2 text-grey-6 text-center">
          {{ data.subtitle }}
        </div>
      </q-item-section>

      <q-item-section class="button-section">
        <q-btn
          class="button"
          label="Ativar"
          text-color="white"
          size="1.2em"
          :icon="data.state ? 'mic_off' : 'mic'"
          :color="data.state ? 'red' : 'green'"
          @click="toggleState(data.state)"
        />
      </q-item-section>
    </q-item>
  </q-card>
</template>

<script lang="ts">
import { defineComponent, PropType } from "vue";
import { Data } from "./types";

export default defineComponent({
  name: "InteractionMenu",

  props: {
    data: {
      type: Object as PropType<Data>,
      required: true,
    },
  },

  methods: {
    toggleState(state: boolean): void {
      this.$emit("updateState", !state);
    },
  },
});
</script>

<style lang="scss" scoped>
@import "src/components/InteractionMenu/styles.scss";
</style>

<template>
  <q-card id="interaction-menu">
    <q-item class="general-content flex column items-center">
      <q-item-section class="avatar-section flex items-center">
        <q-avatar class="avatar">
          <img :src="`${data.avatar}`" alt="Avatar" />
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
import { Notify } from "quasar";
import { Data } from "./types";

export default defineComponent({
  name: "InteractionMenu",

  props: {
    data: {
      type: Object as PropType<Data>,
      required: true,
    },
  },

  data(): { speechApi: any } {
    return {
      speechApi: {},
    };
  },

  mounted() {
    this.initSpeechApi();
  },

  methods: {
    initSpeechApi(): void {
      const SpeechToText =
        window.SpeechRecognition || window.webkitSpeechRecognition;

      this.speechApi = new SpeechToText();
      this.speechApi.continuous = true;
      this.speechApi.lang = "pt-BR";

      this.speechApi.onresult = (event: any) => {
        const { resultIndex } = event;
        const { transcript } = event.results[resultIndex][0];

        Notify.create({
          position: "top",
          message: transcript,
          type: "positive",
          progress: true,
        });
      };
    },

    toggleState(state: boolean): void {
      const toggledState = !state;

      this.$emit("updateState", toggledState);
      this.toggleSpeechRecognition(toggledState);
    },

    toggleSpeechRecognition(state: boolean): void {
      if (state) {
        this.startCapturingSpeech();
        return;
      }

      if (!state) {
        this.stopCapturingSpeech();
      }
    },

    startCapturingSpeech(): void {
      this.speechApi.start();
    },

    stopCapturingSpeech(): void {
      this.speechApi.stop();
    },
  },
});
</script>

<style lang="scss" scoped>
@import "src/components/InteractionMenu/styles.scss";
</style>

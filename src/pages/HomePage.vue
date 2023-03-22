<template>
  <div id="home-page" class="flex justify-center items-center">
    <interaction-menu
      :data="interactionMenuData"
      @updateState="updateState($event)"
    />
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import InteractionMenu from "src/components/InteractionMenu/InteractionMenu.vue";
import { Data } from "src/components/InteractionMenu/types";

export default defineComponent({
  name: "HomePage",

  components: { InteractionMenu },

  data() {
    const interactionMenuData: Data = {
      avatar: "mia-avatar.png",
      title: "Assistente Virtual Mia",
      subtitle: "Ative abaixo:",
      state: false,
    };

    return { interactionMenuData };
  },

  methods: {
    updateState(state: boolean): void {
      this.interactionMenuData.state = state;
    },
  },
});
</script>

<style lang="scss" scoped>
@import "src/css/HomePage/homePage.scss";
</style>

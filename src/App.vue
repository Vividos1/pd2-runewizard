<template>
  <app-header v-if="useLayoutHeader" />

  <main class="rw-Layout-rowContainer rw-Main py-3 flex mb-14">
    <div class="mr-5 rw-RuneSidebar">
      <runes />
    </div>
    <div class="min-w-0 flex-1 rw-RunewordsContent">
      <runewords />
    </div>
  </main>

  <app-footer />
</template>

<script lang="ts">
import { defineComponent } from "vue";

import AppHeader from "@/components/AppHeader.vue";
import AppFooter from "@/components/AppFooter.vue";
import Runes from "@/components/Runes.vue";
import Runewords from "@/components/Runewords.vue";

export default defineComponent({
  name: "App",

  components: {
    AppHeader,
    AppFooter,
    Runes,
    Runewords,
  },

  computed: {
    useLayoutHeader(): boolean {
      return true;
    },
  },
});
</script>

<template>
  <div id="app">
    <FormCmpt />
  </div>
</template>

<script lang="ts">
import { Component, Vue } from "vue-property-decorator";
import FormCmpt from "@/components/FormCmpt.vue";

@Component({ components: { FormCmpt } })
export default class App extends Vue {}
</script>

<style lang="scss">
@import "assets/styles/index";
</style>

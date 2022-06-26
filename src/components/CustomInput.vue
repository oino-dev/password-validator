<template>
  <div class="field">
    <span class="field__caption">{{ label }}</span>
    <template>
      <input
        v-model="model"
        :placeholder="placeholder"
        :type="type"
        class="field__input"
      />
    </template>
    <template>
      <slot></slot>
    </template>
  </div>
</template>

<script lang="ts">
import { Component, Prop, Vue } from "vue-property-decorator";
import { clearingDoubleSpaces } from "@/helpers/formatting";

@Component({})
export default class CustomInput extends Vue {
  @Prop({ required: true }) readonly label!: string;
  @Prop() value!: string | number;
  @Prop() filter?: any;
  @Prop() readonly placeholder?: string;
  @Prop() readonly type?: string;

  emittedValue = this.value;

  get model(): string | number {
    return this.emittedValue;
  }

  set model(value: string | number) {
    this.emittedValue =
      typeof value === "string" ? this.getFilteredValue(value) : value;

    this.$emit("input", value);
  }

  getFilteredValue(value: string): string {
    const filteredValue: string = this.filter ? this.filter(value) : value;

    return clearingDoubleSpaces(filteredValue).trim();
  }
}
</script>

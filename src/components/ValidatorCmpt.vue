<template>
  <div class="validators">
    <span :class="{ 'is-valid': validators.hasLength }">8+</span>
    <span
      :class="{
        'is-valid': validators.hasLatinLowercase,
        'is-non-valid': validators.hasCyrillic,
      }"
      >a-z</span
    >
    <span
      :class="{
        'is-valid': validators.hasLatinUppercase,
        'is-non-valid': validators.hasCyrillic,
      }"
      >A-Z</span
    >
    <span :class="{ 'is-valid': validators.hasNumber }">567</span>
    <span :class="{ 'is-valid': validators.hasSpecChar }">%^&</span>
    <span
      :class="{
        'is-valid': !validators.hasSpace,
        'is-non-valid': validators.hasSpace,
      }"
      >␣</span
    >
  </div>
</template>

<script lang="ts">
import { Component, Prop, Vue } from "vue-property-decorator";
import { IValidator } from "@/components/FormCmpt.vue";

@Component({})
export default class ValidatorCmpt extends Vue {
  @Prop() readonly validators!: IValidator;
}
</script>

<style lang="scss" scoped>
@import "../assets/styles/base/_variable.scss";

.validators {
  display: flex;
  margin: 10px 0 0 10px;
}

span {
  background-color: rgba(0, 0, 0, 0.1);
  border-radius: 1em;
  display: block;
  font-size: 12px;
  font-weight: 500;
  margin-right: 7px;
  padding: 3px 12px;
  transition: ease-in-out all 0.3s;
}

.is-valid {
  background-color: $is-valid-color;
  color: #fff;
  transition: ease-in-out all 0.3s;
}

.is-non-valid {
  background-color: $is-non-valid;
  color: #fff;
  transition: ease-in-out all 0.3s;
}
</style>

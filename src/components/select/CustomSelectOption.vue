<template>
  <span
    :class="[
      'option',
      { 'option-hovered': hover === value },
      { 'option-selected': select === value },
    ]"
    @click="$emit('optionClick', value)"
    @mouseenter="$emit('optionHover', value)"
  >
    {{ value }}
  </span>
</template>

<script>
export default {
  name: "CustomSelectOption",
  props: {
    value: String,
    hover: String,
    select: String,
  },
};
</script>

<style lang="scss">
.option {
  padding: 12px 4px;
  border-radius: 4px;
  &-hovered {
    background-color: #dadada;
  }
  &-selected {
    background-color: #eeeeee;
  }
}
</style>
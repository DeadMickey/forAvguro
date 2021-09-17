<template>
  <div class="select-wrapper">
    <div
      tabindex="0"
      :class="['custom-select', { 'custom-select-disabled': disabled }]"
      @click.stop="changeMenuState"
      @keyup.enter="tabChangeMenuState"
      @keyup.down="changeSelectedItem($event)"
      @keyup.up="changeSelectedItem($event)"
      @blur="blurMenu"
      @focus="menu.onFocus = true"
      ref="customSelect"
    >
      <div class="custom-select-label">{{ label }}</div>
      <div v-if="!disabled" class="custom-select-selected">
        {{ selectedValue ?? "Не выбрано" }}
      </div>
      <div class="custom-select-chevron">
        <i :class="`fal fa-chevron-${arrowIconState}`"></i>
      </div>
      <div
        v-if="menu.show"
        :class="['custom-select-menu', { 'custom-select-menu-top': menu.top }]"
        :style="`max-height:${menu.maxHeight}px`"
      >
        <template v-if="options.flet">
          <div class="flet">
            <CustomSelectOption
              v-for="el in options.flet"
              :key="el"
              :value="el"
              :hover="currentHover"
              :select="selectedValue"
              @optionClick="pointClickOnOption"
              @optionHover="pointHoverOnOption"
            />
          </div>
        </template>
        <template v-if="options.groups">
          <div
            class="group"
            v-for="group in options.groups"
            :key="group.caption"
          >
            <span class="group-caption">{{ group.caption }}</span>
            <CustomSelectOption
              v-for="el in group.fields"
              :key="el"
              :value="el"
              :hover="currentHover"
              :select="selectedValue"
              @optionClick="pointClickOnOption"
              @optionHover="pointHoverOnOption"
            />
          </div>
        </template>
      </div>
    </div>
  </div>
</template>

<script>
import CustomSelectOption from "@/components/select/CustomSelectOption";
export default {
  name: "CustomSelect",
  components: { CustomSelectOption },
  props: {
    id: String,
    label: String,
    options: Object,
    disabled: Boolean,
  },
  data: () => ({
    menu: {
      show: false,
      top: false,
      maxHeight: 550,
      onFocus: false,
    },
    selectedValue: null,
    arrowNumber: 0,
    currentHover: null,
  }),
  computed: {
    nativeSelectOptions() {
      let fletOptions = [];
      let groupOptions = [];

      if (this.options.flet) {
        this.options.flet.forEach((item) => {
          fletOptions.push(item);
        });
      }

      if (this.options.groups) {
        this.options.groups.forEach((item) => {
          groupOptions.push(...item.fields);
        });
      }

      return [].concat(fletOptions, groupOptions);
    },
    arrowIconState() {
      return this.menu.show ? "up" : "down";
    },
    sameValues() {
      return this.currentHover === this.selectedValue;
    },
  },
  methods: {
    changeSelectedItem(event) {
      if (
        event.key === "ArrowDown" &&
        this.arrowNumber < this.nativeSelectOptions.length
      ) {
        ++this.arrowNumber;
      } else if (event.key === "ArrowUp" && this.arrowNumber) {
        --this.arrowNumber;
      }
      if (this.menu.onFocus && !this.menu.show) {
        this.selectedValue = this.nativeSelectOptions[this.arrowNumber - 1];
        this.currentHover = this.nativeSelectOptions[this.arrowNumber - 1];
      } else {
        this.currentHover = this.nativeSelectOptions[this.arrowNumber - 1];
      }
    },
    changeMenuState() {
      if (!this.disabled) {
        this.menu.show = !this.menu.show;
        const customSelectOffset = this.$refs.customSelect.offsetTop;
        const customSelectHeight = this.$refs.customSelect.clientHeight;
        const fullSelectHeight = this.menu.maxHeight - customSelectHeight;

        if (window.innerHeight - customSelectOffset < fullSelectHeight) {
          this.menu.top = true;
        } else {
          this.menu.top = false;
        }
      }
    },
    tabChangeMenuState() {
      this.changeMenuState();
      this.selectedValue = this.currentHover;
    },
    pointClickOnOption(value) {
      this.selectedValue = value;
    },
    pointHoverOnOption(value) {
      this.currentHover = value;
    },
    blurMenu() {
      this.menu.show = false;
      this.menu.onFocus = false;
    },
  },
  mounted() {
    this.selectedValue = this.nativeSelectOptions[0];
  },
};
</script>

<style scoped lang="scss">
.custom-select {
  display: grid;
  grid-template-columns: 1fr auto;
  grid-template-rows: auto auto;
  position: relative;
  min-width: 250px;
  padding: 5px 15px;
  text-align: left;
  border: 1px solid #ccc;
  border-radius: 10px;
  cursor: pointer;
  &:focus {
    border-color: #1bc97a;
    box-shadow: 0 5px 10px -5px #1bc97a;
    outline: none;
  }
  &-label {
    font-size: 14px;
    color: #7e7e7e;
  }
  &-selected {
    color: black;
  }
  &-chevron {
    display: flex;
    align-items: center;
    grid-column: 2;
    grid-row: 1 / -1;
  }
  &-menu {
    position: absolute;
    overflow: hidden;
    overflow-y: auto;
    top: calc(100% + 8px);
    width: 100%;
    border: inherit;
    box-shadow: 0 10px 15px -5px #a2a2a2;
    border-radius: inherit;
    &-top {
      top: initial;
      bottom: calc(100% + 8px);
    }
    .flet,
    .group {
      display: flex;
      flex-flow: column;
      margin: 8px 0;
      padding: 8px;
    }
    .group {
      border-top: 1px solid grey;
      &-caption {
        font-weight: 700;
        margin-bottom: 8px;
      }
    }
  }
  &-disabled {
    padding: 15px;
    background-color: rgb(226, 226, 226);
    opacity: 0.5;
    cursor: default;
    &:focus {
      border-color: inherit;
      box-shadow: none;
    }
  }
}
</style>

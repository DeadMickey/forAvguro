<template>
  <CustomSelect :options="testData" label="label" id="select_1"/>
</template>

<script>
import CustomSelect from "@/components/select/CustomSelect";
export default {
  name: "App",
  components: {
    CustomSelect,
  },
  data: () => ({
    testData: {
      flet: ["Filed 1", "Field 2"],
      groups: [
        {
          caption: "Caption A",
          fields: ["Filed A1", "Field A2", "Field A3"],
        },
        {
          caption: "Caption B",
          fields: ["Filed B1", "Field B2", "Field B3"],
        },
      ],
    },
  }),
};
</script>

<style lang="scss">
@import url("assets/fontawesome/all.min.css");
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  display: flex;
  justify-content: center;
}
* {
  box-sizing: border-box;
}
body {
  margin: 0;
  padding: 0;
}
</style>

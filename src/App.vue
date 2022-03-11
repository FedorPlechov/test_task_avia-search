<template>
  <TheViewFlights />
</template>

<script>
import TheViewFlights from "@/components/TheViewFlights";
export default {
  name: "App",
  components: {
    TheViewFlights,
  },
};
</script>

<style lang="scss">
@import url("./assets/style/normalize.css");
@import url("./assets/style/icon-font.css");

body {
  height: 100vh;
}
#app {
  height: 100%;
  width: 100%;
}
</style>

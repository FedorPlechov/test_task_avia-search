<template>
  <div class="container__main">
    <TheFilterForm />
    <TheListOfFlights />
  </div>
</template>

<script>
import TheFilterForm from "@/components/filterForm/TheFilterForm";
import TheListOfFlights from "@/components/flights/TheListOfFlightCards";

export default {
  name: "TheViewFlights",
  components: {
    TheListOfFlights,
    TheFilterForm,
  },
};
</script>

<style lang="scss" scoped>
.container__main {
  display: flex;
  max-width: 925px;
  max-height: 750px;
  padding: 60px 0px;
  margin: auto;
  @media (max-width: 45rem) {
    flex-direction: column;
  }
}
</style>

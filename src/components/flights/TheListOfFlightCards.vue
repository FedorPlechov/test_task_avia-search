<template>
  <div class="container__flight">
    <CardFlight
      v-for="flight of showFlights"
      :key="flight.id"
      :flight="flight"
    />
    <button
      v-show="filteredFlights.length > showFlights.length"
      class="show-more"
      @click="page++"
    >
      Показать еще
    </button>
  </div>
</template>

<script>
import CardFlight from "@/components/flights/CardFlight";

export default {
  name: "TheListOfFlightCards",

  components: {
    CardFlight,
  },
  props: {
    flights: {
      type: Array,
      required: true,
    },
    sort: String,
    priceFilter: Object,
    numbersOfConnections: Array,
    chosenAirlines: Array,
  },
  data() {
    return {
      page: 1,
    };
  },
  computed: {
    filteredFlights() {
      const result = this.sortFlights().filter((el) => {
        return (
          this.priceFilter.start < el.price &&
          this.priceFilter.end > el.price &&
          this.filterConnectionsCompanies(el) &&
          this.filterChosenAirlines(el)
        );
      });
      this.$emit("filteredFlights", result);
      return result;
    },
    showFlights() {
      return this.filteredFlights.slice(0, this.page * 2);
    },
  },
  methods: {
    filterChosenAirlines(el) {
      if (!this.chosenAirlines.length) {
        return true;
      }
      return this.chosenAirlines.some((item) => item === el.carrier);
    },
    filterConnectionsCompanies(el) {
      const elNumbersOfConnections =
        +el.flyTo.hasConnectionFlight + +el.flyBack.hasConnectionFlight;
      return this.numbersOfConnections.length === 0
        ? true
        : this.numbersOfConnections.length === 1
        ? elNumbersOfConnections === +this.numbersOfConnections[0]
        : elNumbersOfConnections === +this.numbersOfConnections[0] ||
          elNumbersOfConnections === +this.numbersOfConnections[1];
    },
    sortFlights() {
      const copyFlights = [...this.flights];
      if (this.sort === "increase") {
        return copyFlights.sort((a, b) => {
          return a.price - b.price;
        });
      }
      if (this.sort === "decrease") {
        return copyFlights.sort((a, b) => {
          return b.price - a.price;
        });
      }
      if (this.sort === "duration") {
        return copyFlights.sort((a, b) => {
          return (
            a.flyTo.duration +
            a.flyBack.duration -
            (b.flyTo.duration + b.flyBack.duration)
          );
        });
      }
    },
  },
};
</script>

<style lang="scss" scoped>
.container__flight {
  width: 73%;
  display: flex;
  flex-direction: column;
  margin-right: 25px;
  @media (max-width: 45rem) {
    margin-right: 0;
    width: 100%;
  }

  .show-more {
    width: 40%;
    align-self: center;
    margin: -11px 0 20px 9px;
    font-size: 1.1rem;
    padding: 2px 15px 2px 0;
    cursor: pointer;
  }
}
</style>

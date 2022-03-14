<template>
  <div class="container__main">
    <TheFilterForm
      @sort="setSort($event)"
      @startPrice="priceFilter.start = $event"
      @endPrice="priceFilter.end = $event"
      @airlineFilter="chosenAirlines = $event"
      @filterConnections="numbersOfConnections = $event"
      :all-airlines="allAirlinesBestPrice"
      :filtered-flights="filteredFlights"
    />
    <TheListOfFlights
      :flights="flights"
      :sort="sort"
      :price-filter="priceFilter"
      :numbersOfConnections="numbersOfConnections"
      :chosen-airlines="chosenAirlines"
      @filteredFlights="filteredFlights = $event"
    />
  </div>
</template>

<script>
import TheFilterForm from "@/components/filterForm/TheFilterForm";
import TheListOfFlights from "@/components/flights/TheListOfFlightCards";
import dataFlights from "@/assets/data/flights.json";

export default {
  name: "TheViewFlights",
  components: {
    TheListOfFlights,
    TheFilterForm,
  },
  data() {
    return {
      dataFlights: dataFlights,
      sort: "increase",
      flights: null,
      numbersOfConnections: [],
      priceFilter: {
        start: 0,
        end: 1000000,
      },
      chosenAirlines: [],
      filteredFlights: [],
    };
  },
  computed: {
    allAirlinesBestPrice() {
      const arr = [];
      const companies = this.flights
        .map((el) => {
          return {
            carrier: el.carrier,
            price: el.price,
            numbersOfConnections:
              +el.flyTo.hasConnectionFlight + +el.flyBack.hasConnectionFlight,
            duration: el.flyTo.duration + el.flyBack.duration,
          };
        })
        .filter((el) => {
          return (
            this.priceFilter.start < el.price &&
            this.priceFilter.end > el.price &&
            this.filterConnectionsCompanies(el)
          );
        });
      return this.sortCompanies(companies).filter((item) =>
        this.itemCheckBestPriceAllCompanies(item, arr)
      );
    },
  },
  methods: {
    sortCompanies(arr) {
      if (this.sort === "increase") {
        return arr.sort((a, b) => {
          return a.price - b.price;
        });
      }
      if (this.sort === "decrease") {
        return arr.sort((a, b) => {
          return b.price - a.price;
        });
      }
      if (this.sort === "duration") {
        return arr.sort((a, b) => {
          return a.duration - b.duration;
        });
      }
    },
    filterConnectionsCompanies(el) {
      return this.numbersOfConnections.length === 0
        ? true
        : this.numbersOfConnections.length === 1
        ? el.numbersOfConnections === +this.numbersOfConnections[0]
        : el.numbersOfConnections === +this.numbersOfConnections[0] ||
          el.numbersOfConnections === +this.numbersOfConnections[1];
    },
    itemCheckBestPriceAllCompanies(item, arr) {
      if (arr.indexOf(item.carrier) === -1) {
        arr.push(item.carrier);
        return true;
      }
      return false;
    },
    setSort(payload) {
      this.sort = payload;
    },
    getFlights() {
      const arr = [];
      this.dataFlights.result.flights.forEach((el) => {
        const segmentsTo = el.flight.legs[0].segments;
        const segmentsBack = el.flight.legs[1].segments;
        arr.push({
          id: Date.now() * Math.random(),
          carrier: el.flight.carrier.caption,
          price: el.flight.price.total.amount,
          flyTo: {
            duration: el.flight.legs[0].duration,
            ...this.getObjectInformFromSegments(segmentsTo),
          },
          flyBack: {
            duration: el.flight.legs[1].duration,
            ...this.getObjectInformFromSegments(segmentsBack),
          },
        });
      });
      return arr;
    },
    getObjectInformFromSegments(segments) {
      return {
        hasConnectionFlight: segments.length > 1,
        departureCityAndAirport: segments[0].departureCity
          ? segments[0].departureCity.caption +
            ", " +
            segments[0].departureAirport.caption
          : "" + ", " + segments[0].departureAirport.caption,
        departureAirportUid: "(" + segments[0].departureAirport.uid + ")",
        departureDate: segments[0].departureDate,
        arrivalCityAndAirport: segments[segments.length - 1].arrivalCity
          ? segments[segments.length - 1].arrivalCity.caption +
            ", " +
            segments[segments.length - 1].arrivalAirport.caption
          : "" + ", " + segments[segments.length - 1].arrivalAirport.caption,
        arrivalAirportUid:
          "(" + segments[segments.length - 1].arrivalAirport.uid + ")",
        arrivalDate: segments[segments.length - 1].arrivalDate,
        operatingAirline: segments[segments.length - 1].operatingAirline
          ? segments[segments.length - 1].operatingAirline.caption
          : segments[segments.length - 1].airline.caption,
      };
    },
  },
  beforeMount() {
    this.flights = this.getFlights();
  },
};
</script>

<style lang="scss" scoped>
.container__main {
  display: flex;
  max-width: 925px;
  padding: 60px 0px;
  margin: auto;
  @media (max-width: 45rem) {
    flex-direction: column;
    padding-top: 0;
  }
}
</style>

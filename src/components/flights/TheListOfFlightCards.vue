<template>
  <div class="container__flight">
    <CardFlight
      v-for="flight of showFlights"
      :key="flight.id"
      :flight="flight"
    />
    <button
      v-show="flights.length > showFlights.length"
      class="show-more"
      @click="page++"
    >
      Показать еще
    </button>
  </div>
</template>

<script>
import CardFlight from "@/components/flights/CardFlight";
import dataFlights from "@/assets/data/flights.json";

export default {
  name: "TheListOfFlightCards",

  components: {
    CardFlight,
  },
  data() {
    return {
      page: 1,
      dataFlights: dataFlights,
    };
  },
  computed: {
    showFlights() {
      return this.flights.slice(0, this.page * 2);
    },
    flights() {
      const arr = [];
      this.dataFlights.result.flights.forEach((el) => {
        const segmentsTo = el.flight.legs[0].segments;
        const segmentsBack = el.flight.legs[1].segments;
        arr.push({
          id: el.flightToken,
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
  },
  methods: {
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
};
</script>

<style lang="scss" scoped>
.container__flight {
  width: 73%;
  height: 20%;
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

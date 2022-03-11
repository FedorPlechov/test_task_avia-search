<template>
  <div class="header__flight">
    <h3>{{ flight.carrier }}</h3>
    <div class="price">
      <p class="icon-ruble-sign">{{ price }}</p>
      <p>Стоимость для одного взрослого пассажира</p>
    </div>
  </div>
  <OneFlight :flight-inform="flight.flyTo" />
  <div class="border"></div>
  <OneFlight :flight-inform="flight.flyBack" />
  <button class="choose-button" @click="chooseFlight">ВЫБРАТЬ</button>
</template>

<script>
import OneFlight from "@/components/flights/OneFlight";

export default {
  name: "CardFlight",
  components: {
    OneFlight,
  },
  props: ["flight"],
  computed: {
    price() {
      return this.flight.price + " ";
    },
  },
  methods: {
    chooseFlight() {
      alert("Рейс выбран");
    },
  },
};
</script>

<style lang="scss" scoped>
.header__flight {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  align-items: center;
  background: rgba(32, 133, 234, 0.97);
  color: white;
  padding: 0 16px;

  h3 {
    font-size: 0.8rem;
    font-weight: 500;
    line-height: 1rem;
  }

  .price {
    text-align: right;
    flex: 1 1 70%;

    p {
      padding: 0;
      margin: 0;
      font-size: 1.2rem;
    }

    p:last-of-type {
      font-size: 0.6rem;
    }

    .icon-ruble-sign:after {
      font-size: 1rem;
    }
  }
}

.border {
  border-top: 2px solid #67a1f1;
}

.choose-button {
  display: inline-flex;
  justify-content: center;
  padding: 7px 0 10px 0;
  color: white;
  border: none;
  background: burlywood;
  box-shadow: 0px -5px 0 burlywood;
  margin-bottom: 27px;
  letter-spacing: -1px;
  cursor: pointer;
  transition: all 200ms ease;

  &:hover {
    background: rgba(222, 185, 137, 0.92);
    color: #b66606;
  }

  &:active {
    transform: scale(0.9);
  }
}
</style>

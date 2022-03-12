<template>
  <form class="container__filter" @submit.prevent>
    <div class="container__mobile">
      <div class="container__sort">
        <h3>Сортировать</h3>
        <label
          ><input
            checked
            type="radio"
            name="sort"
            value="increase"
            @input="$emit('sort', $event.target.value)"
          />
          - по возврастанию цены</label
        >
        <label
          ><input
            type="radio"
            name="sort"
            value="decrease"
            @input="$emit('sort', $event.target.value)"
          />
          - по убыванию цены</label
        >
        <label
          ><input
            type="radio"
            name="sort"
            value="duration"
            @input="$emit('sort', $event.target.value)"
          />
          - по времени в пути</label
        >
      </div>
      <div class="container__filter-flights">
        <h3>Фильтровать</h3>

        <label v-if="isShowCheckboxOne"
          ><input
            type="checkbox"
            name="one"
            value="1"
            v-model="numbersOfConnections"
            @change="$emit('filterConnections', numbersOfConnections)"
          />
          - 1 пересадка</label
        >
        <div v-else class="empty"></div>

        <label v-if="isShowCheckboxZero"
          ><input
            type="checkbox"
            name="zero"
            value="0"
            v-model="numbersOfConnections"
            @change="$emit('filterConnections', numbersOfConnections)"
          />
          - без пересадок</label
        >
        <div v-else class="empty"></div>
      </div>
    </div>
    <div class="container__mobile column">
      <div class="container__price">
        <h3>Цена</h3>
        <label
          >От
          <input
            type="number"
            v-model="startPrice"
            @input="$emit('startPrice', startPrice)"
        /></label>
        <label
          >До
          <input
            type="number"
            v-model="endPrice"
            @input="$emit('endPrice', endPrice)"
        /></label>
      </div>
      <div class="container__companies">
        <h3>Авиакомпании</h3>
        <label v-for="airline of showAirlines" :key="airline.carrier"
          ><input
            type="checkbox"
            :value="airline.carrier"
            v-model="chosenAirlines"
            @change="$emit('airlineFilter', chosenAirlines)"
          />
          - {{ airline.carrier }} от {{ airline.price }} р.</label
        >
      </div>
    </div>
  </form>
</template>

<script>
export default {
  name: "TheFilterForm",
  props: ["allAirlines", "filteredFlights"],
  data() {
    return {
      startPrice: 0,
      endPrice: 1000000,
      numbersOfConnections: [],
      chosenAirlines: [],
    };
  },
  computed: {
    showAirlines() {
      return this.allAirlines;
    },
    allNumbersOfConnections() {
      return this.filteredFlights
        .filter((el) => {
          return this.filterChosenAirlines(el);
        })
        .map((el) => {
          return el.flyTo.hasConnectionFlight + el.flyBack.hasConnectionFlight;
        });
    },
    isShowCheckboxOne() {
      return this.allNumbersOfConnections.some((el) => {
        return el === 1;
      });
    },
    isShowCheckboxZero() {
      return this.allNumbersOfConnections.some((el) => {
        return el === 0;
      });
    },
  },
  methods: {
    filterChosenAirlines(el) {
      if (!this.chosenAirlines.length) {
        return true;
      }
      return this.chosenAirlines.some((item) => item === el.carrier);
    },
  },
};
</script>

<style lang="scss" scoped>
.container__filter {
  width: 30%;
  margin-left: 5px;
  @media (max-width: 45rem) {
    width: 100%;
    display: flex;
    flex-direction: column;
    align-items: center;
  }
}

.container__mobile {
  @media (max-width: 45rem) {
    display: flex;
    width: 100%;
    justify-content: center;
  }

  &.column {
    @media (max-width: 45rem) {
      flex-direction: column;
      justify-content: center;
      align-items: center;
      padding-bottom: 5px;
    }
  }
}

.container__sort {
  display: flex;
  flex-direction: column;
  padding: 14px 0 33px 0;
  @media (max-width: 45rem) {
    margin: 0;
    flex: 0 1 40%;
    padding: 14px 5px 5px 5px;
  }
}

.container__filter-flights {
  display: flex;
  flex-direction: column;
  @media (max-width: 45rem) {
    padding: 13px 0 0 0;
  }

  h3 {
    @media (max-width: 45rem) {
      padding: 0 0 20px 0;
    }
  }

  label:last-of-type {
    margin-top: 2px;
  }
}

.container__price {
  display: flex;
  flex-direction: column;
  padding: 5px 0 0 2px;
  @media (max-width: 45rem) {
    padding: 0 0 0 10px;
    width: 65%;
    flex-direction: row;
    justify-content: center;
  }

  h3 {
    @media (max-width: 45rem) {
      margin: 2px 5px 0 0;
    }
  }

  label:last-of-type {
    margin-top: 20px;
    @media (max-width: 45rem) {
      margin-top: 0;
    }
  }

  label {
    input {
      width: 65%;
    }
  }
}
.empty {
  height: 17px;
  width: 100%;
}

.container__companies {
  display: flex;
  flex-direction: column;
}

h3 {
  font-size: 0.8rem;
  padding-bottom: 4px;
}

label {
  font-size: 0.8rem;
}
</style>

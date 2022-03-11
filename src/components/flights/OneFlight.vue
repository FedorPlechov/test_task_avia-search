<template>
  <div class="container__inform">
    <div class="from-to padding-block">
      {{ flightInform.departureCityAndAirport }}
      <span class="color-blue icon-arrow"
        >{{ flightInform.departureAirportUid }}
      </span>
      {{ flightInform.arrivalCityAndAirport }}
      <span class="color-blue">{{ flightInform.arrivalAirportUid }}</span>
    </div>
    <div class="container__flight-time padding-block">
      <div class="time-departure">
        <span>{{ departureTime }}</span>
        <span class="color-blue">{{ departureDate }}</span>
      </div>
      <div class="time-duration icon-time">{{ duration }}</div>
      <div class="time-arrival">
        <span class="color-blue">{{ arrivalDate }}</span>
        <span>{{ arrivalTime }}</span>
      </div>
    </div>
    <div class="container__connecting-flight">
      <div class="container__content">
        <div class="line"></div>
        <div v-if="flightInform.hasConnectionFlight" class="connection-flight">
          1 пересадка
        </div>
        <div v-else class="line"></div>
        <div class="line"></div>
      </div>
    </div>
    <div class="container__airline padding-block">
      Рейс выполняет: {{ flightInform.operatingAirline }}
    </div>
  </div>
</template>

<script>
import { DateTime } from "luxon";

export default {
  name: "TheOneFlight",
  props: {
    flightInform: {
      type: Object,
    },
  },
  computed: {
    departureStampTime() {
      return DateTime.fromISO(this.flightInform.departureDate);
    },
    arrivalStampTime() {
      return DateTime.fromISO(this.flightInform.arrivalDate);
    },
    duration() {
      return `${Math.floor(this.flightInform.duration / 60)} ч ${
        this.flightInform.duration % 60
      } мин`;
    },
    departureTime() {
      return this.getTime(this.departureStampTime);
    },
    arrivalTime() {
      return this.getTime(this.arrivalStampTime);
    },
    departureDate() {
      return `${this.getDate(this.departureStampTime)} ${this.getWeekDay(
        this.departureStampTime
      )}`;
    },
    arrivalDate() {
      return `${this.getDate(this.arrivalStampTime)} ${this.getWeekDay(
        this.arrivalStampTime
      )} `;
    },
  },
  methods: {
    getTime(dt) {
      return dt.toLocaleString(DateTime.TIME_24_SIMPLE);
    },
    getDate(dt) {
      return dt.toLocaleString({ day: "numeric", month: "short" });
    },
    getWeekDay(dt) {
      return dt.toLocaleString({ weekday: "short" });
    },
  },
};
</script>

<style lang="scss" scoped>
.container__inform {
  padding: 0 16px;

  .from-to {
    border-bottom: 0.5px solid rgba(215, 210, 210, 0.98);

    .icon-arrow:after {
      font-size: 0.7rem;
    }
  }

  .container__flight-time {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding-right: 20px;
    font-size: 1.1rem;

    * > .color-blue {
      font-size: 0.9rem;
    }

    .time-duration {
      transform: translateY(3px) translateX(8px);
      font-size: 1rem;
      @media (max-width: 45rem) {
        transform: none;
        font-size: 0.7rem;
      }
    }

    .time-departure {
      @media (max-width: 45rem) {
        display: flex;
        flex-direction: column;
      }
    }

    .time-arrival {
      @media (max-width: 45rem) {
        display: flex;
        flex-direction: column-reverse;
      }
    }

    .icon-time:before {
      font-size: 1.1rem;
    }
  }
}

.container__connecting-flight {
  display: flex;
  justify-content: center;
  align-items: center;

  .container__content {
    display: flex;
    width: 90%;
    justify-content: flex-start;
    align-items: center;

    .line {
      border-top: 1px solid gray;
      flex: 1 1;
    }

    .line:last-of-type {
      flex: 1 1;
      position: relative;
    }

    .line:last-of-type:after {
      z-index: 100;
      position: absolute;
      display: block;
      content: "";
      width: 10%;
      background: white;
      transform: rotate(180deg);
      height: 3px;
      right: 0;
      top: -2px;
    }

    .connection-flight {
      color: red;
      font-size: 0.9rem;
      padding: 0 10px;
      transform: translateY(-5px);
    }
  }
}

.container__airline {
  font-size: 0.9rem;
}

.padding-block {
  padding: 12px 2px;
}

.color-blue {
  color: #67a1f1;
}
</style>

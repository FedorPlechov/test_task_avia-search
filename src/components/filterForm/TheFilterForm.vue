<template>
  <div class="container__filter">
    <div>
      <div class="container__sort">
        <h3>Сортировать</h3>
        <label><input checked type="radio"/> - по возврастанию цены</label>
        <label><input type="radio"/> - по убыванию цены</label>
        <label><input type="radio"/> - по времени в пути</label>
      </div>
      <div class="container__filter-flights">
        <h3>Фильтровать</h3>
        <label><input type="checkbox"/> - 1 пересадка</label>
        <label><input type="checkbox"/> - без пересадок</label>
      </div>
    </div>
    <div>
      <div class="container__price">
        <h3>Цена</h3>
        <label>От <input type="number"></label>
        <label>До <input type="number"></label>
      </div>
      <div class="container__companies">
        <h3>Авиакомпании</h3>
        <label><input type="checkbox"/> - LOT POLISH Airlines от 21049 р.</label>
        <label><input type="checkbox"/> - Аэрофлот - рос....  от 31733 р.</label>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "TheFilterForm",
};
</script>

<style lang="scss" scoped>
.container__filter {
  width: 30%;
  @media (max-width: 45rem) {
    width: 100%;
    display: flex;
  }
}

.container__sort {
  display: flex;
  flex-direction: column;
  padding: 14px 0 33px 0;
  @media (max-width: 45rem) {
    padding: 20px 10px;

  }
}

.container__filter-flights {
  display: flex;
  flex-direction: column;

  label:last-of-type {
    margin-top: 2px;
  }
}

.container__price {
  display: flex;
  flex-direction: column;
  padding: 5px 0 0 2px;

  label:last-of-type {
    margin-top: 20px;
  }

  label {
    input {
      width: 65%;
    }
  }
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

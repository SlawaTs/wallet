<template>
  <div class="container">
    <div class="row">
      <div class="col" v-for="(item, index) in currencyCardList" :key="index">
        <Card :item="item" :user="user" />
      </div>
    </div>
  </div>
</template>
<script>
import Card from '../components/Card'
export default {
  components: {
    Card,
  },
  data() {
    return {
      currencyCardList: [
        {
          name: 'BTC',
          type: 'Криптовалюта',
          commission: '5%',
          min: '0.001',
          flagCommis: true,
        },
        {
          name: 'USD',
          type: 'Фиатная',
          commission: '5%',
          min: '100',
          flagCommis: true,
        },
        {
          name: 'DOGE',
          type: 'Криптовалюта',
          commission: '0.5 DOGE',
          min: '5',
          flagCommis: false,
          activeIn: false,
          activeOut: false,
        },
        {
          name: 'LTC',
          type: 'Криптовалюта',
          commission: '0.5 LTC',
          min: '1',
          flagCommis: false,
          activeIn: false,
          activeOut: false,
        },
        {
          name: 'SHIB',
          type: 'Криптовалюта',
          commission: '10 SHIB',
          min: '500',
          flagCommis: false,
          activeIn: false,
          activeOut: false,
        },
        {
          name: 'RUR',
          type: 'Фиатная',
          commission: '0%',
          min: '10000',
          flagCommis: true,
          activeIn: false,
          activeOut: false,
        },
        {
          name: 'BNB',
          type: 'Криптовалюта',
          commission: '0.01 BNB',
          min: '0.15',
          flagCommis: false,
          activeIn: false,
          activeOut: false,
        }
      ],


      user: {
        name: 'Имя',
        wallets: [
          {
            type: 'BTC',
            balance: 0,
          },
          {
            type: 'USD',
            balance: 0,
          },
          {
            type: 'DOGE',
            balance: 0,
          }
        ]
      }

    }
  }
}
</script>
<style>
.container {
  max-width: 1400px;
}

.row {
  display: flex;
  flex-wrap: wrap;
}

.col {
  width: 100%;
}
@media (min-width: 576px) {
  .col {
    width: 50%;
  }
}
@media (min-width: 768px) {
  .col {
    width: 33%;
  }
}





.card {
  height: 100%;
}
.wallet{
  margin: 10px;
  background-color: #4d5b5c;
  color: aliceblue;
  height: 100px;

}
.wallet-balance{
  display: flex;
  font-size: 40px;
}
.wallet-balance__name{
  margin: 10px;
}
.wallet-balance__amount{
  margin: 10px;
}
.wallet-control{
  width: 100%;
  display: flex;
  justify-content: space-around;
}

.btn {
  border: none;
}

.wallet-control__in{
  max-width: 80px;
  width: 100%;
}
.wallet-control__out{
  max-width: 80px;
  width: 100%;
}
.form {
  margin: 10px;
  position: relative;
}
.form__sum{

}
.form__mail{
  margin-top: 5px;
  box-sizing: border-box;
}
.form__comeents{
  margin-top: 5px;
  height: 60px;
  box-sizing: border-box;
}
.form__btn1{
  max-width: 80px;
  width: 100%;
}
.form__btn2{
  max-width: 80px;
  width: 100%;
}
@media screen and (max-width: 1050px) {
  .card{
  flex: 0 0 50%;
  }
}
@media screen and (max-width: 680px) {
  .card{
    flex: 0 0 100%;
  }
  }

</style>

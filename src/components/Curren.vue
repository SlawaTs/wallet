<template>
  <div class="conten">
    <div class="card" v-for="(item, index) in info" :key="index">
      <div class="wallet">
        <div class="wallet-balance">
          <div class="wallet-balance__name">{{item.name}}</div>
          <div class="wallet-balance__amount">{{item.balance}}</div>
        </div>
        <div class="wallet-control">
          <input type="button" value="Ввод" class="wallet-control__in"
                 @click="activeFormIn(index)" >
          <input type="button" value="Вывод" class="wallet-control__out"
                 @click="activeFormOut(index)" >
        </div>
      </div>
      <div class="form"
           :class="{active: item.activeIn || item.activeOut}" >
        <input type="text" class="form__sum" placeholder="Сумма"
               v-model="isInput"
               @input="v$.isInput.$touch()">
        <span>Комиссия: {{item.commission}}</span>
        <br>
        <span class="form__valid">{{isValid}}</span>
        <input type="text" class="form__mail" placeholder="Адрес"
               :class="{active: (item.type == 'Криптовалюта')}"
               v-model="requisites"
               @input="v$.requisites.$touch()">
        <input type="text" class="form__mail" placeholder="Реквизиты"
               :class="{active: (item.type == 'Фиатная')}"
               v-model="requisites"
               @input="v$.requisites.$touch()">

        <textarea class="form__comeents" placeholder="Комментарий"></textarea>
        <input type="button" class="form__btn1" value="Ввод"
               @click="deposit(index)"
               :class="{active: item.activeIn}"
               :disabled="v$.$invalid">
        <input type="button" class="form__btn2" value="Вывод"
               @click="withdraw(index)"
               :class="{active: item.activeOut}"
               :disabled="v$.$invalid">
      </div>
    </div>
  </div>
</template>
<script>
import useVuelidate from '@vuelidate/core'
import { required } from '@vuelidate/validators'
export default {
  setup () {
    return { v$: useVuelidate() }
  },
  data(){
    return {
      requisites: '',
      isInput: '',
      isValid: '',
      info: [
        {
          name: 'BTC',
          type: 'Криптовалюта',
          commission: '5%',
          min: '0.001',
          balance: '0',
          flagCommis: true,
          activeIn: false,
          activeOut: false,
        },
        {
          name: 'USD',
          type: 'Фиатная',
          commission: '5%',
          min: '100',
          balance: '0',
          flagCommis: true,
          activeIn: false,
          activeOut: false,
        },
        {
          name: 'DOGE',
          type: 'Криптовалюта',
          commission: '0.5 DOGE',
          min: '5',
          balance: '0',
          flagCommis: false,
          activeIn: false,
          activeOut: false,
        },
        {
          name: 'LTC',
          type: 'Криптовалюта',
          commission: '0.5 LTC',
          min: '1',
          balance: '0',
          flagCommis: false,
          activeIn: false,
          activeOut: false,
        },
        {
          name: 'SHIB',
          type: 'Криптовалюта',
          commission: '10 SHIB',
          min: '500',
          balance: '0',
          flagCommis: false,
          activeIn: false,
          activeOut: false,
        },
        {
          name: 'RUR',
          type: 'Фиатная',
          commission: '0%',
          min: '10000',
          balance: '0',
          flagCommis: true,
          activeIn: false,
          activeOut: false,
        },
        {
          name: 'BNB',
          type: 'Криптовалюта',
          commission: '0.01 BNB',
          min: '0.15',
          balance: '0',
          flagCommis: false,
          activeIn: false,
          activeOut: false,
        }
      ]
    }
  },
  validations(){
    return{
      requisites:{required,alpha: val =>  /^[0-9]*$/i.test(val)},
      isInput:{required,alpha: val => /^([0-9]*[.]?[0-9]+)$/i.test(val)}
    }
  },
  methods:{
    deposit(index){
      if (parseFloat(this.isInput) >= parseFloat(this.info[index].min)){
        if (this.info[index].flagCommis) {
          this.info[index].balance = parseFloat(this.info[index].balance) + (parseFloat(this.isInput) - (parseFloat(this.isInput) *(parseFloat(this.info[index].commission))/100));
          this.isInput = '';
          this.isValid = '';
          this.requisites = '';
        }else {
          this.info[index].balance = parseFloat(this.info[index].balance) + parseFloat(this.isInput) - parseFloat(this.info[index].commission);
          this.isInput = '';
          this.isValid = '';
          this.requisites = '';
        }
      }else this.isValid = 'Введенное значение меньше минимального!'
    },
    withdraw(index){
      if (this.info[index].flagCommis){
        if ((parseFloat(this.info[index].balance) - (parseFloat(this.isInput) + (parseFloat(this.isInput) * parseFloat(this.info[index].commission)/100))) >= 0 ){
          this.info[index].balance = parseFloat(this.info[index].balance) - (parseFloat(this.isInput) + (parseFloat(this.isInput) * parseFloat(this.info[index].commission)/100));
          this.isValid = '';
          this.isInput = '';
          this.requisites = '';
        }else this.isValid = 'Введенное значение меньше вашего баланса!'
      }else {
        if (parseFloat(this.info[index].balance) - (parseFloat(this.isInput) + parseFloat((this.info[index].commission))) >= 0 ){
          this.info[index].balance = parseFloat(this.info[index].balance) - (parseFloat(this.isInput) + parseFloat(this.info[index].commission));
          this.isInput = '';
          this.isValid = '';
          this.requisites = '';
        }else this.isValid = 'Введенное значение меньше вашего баланса!'
      }
    },
    activeFormIn(index){
      this.isInput = '';
      this.isValid = '';
      this.requisites = '';
      for(let i = 0; i < this.info.length; i++){
        if (index == i){
          if (this.info[index].activeIn){
            this.info[index].activeIn = false
          }else {
            this.info[index].activeIn = true;
            this.info[index].activeOut = false;
          }
        }else {
          this.info[i].activeIn = false;
          this.info[i].activeOut = false;
        }
      }
    },
    activeFormOut(index){
      this.isInput = '';
      this.isValid = '';
      this.requisites = '';
      for (let i = 0; i < this.info.length; i++) {
        if (index == i) {
          if (this.info[index].activeOut){
            this.info[index].activeOut = false;
          }else {
            this.info[index].activeIn = false;
            this.info[index].activeOut = true;
        }
        }else {
          this.info[i].activeIn = false;
          this.info[i].activeOut = false;
        }
      }
  }
},}
</script>
<style>
.conten{
  display: flex;
  flex-wrap: wrap;
  max-width: 1400px;
  width: 100%;
}
.card{
  max-width: 430px;
  flex: 0 0 32%;
}
.wallet{
  width: 100%;
  margin: 10px;
  max-width: 330px;
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
}
.wallet-control__in{
  margin-left: 15%;
  border: none;
  max-width: 80px;
  width: 100%;
}
.wallet-control__out{
  margin-left: 20%;
  border: none;
  max-width: 80px;
  width: 100%;
}
.form {
  max-width: 300px;
  width: 100%;
  margin: 10px;
  position: relative;
  display: none;
}
.form__sum{

}
.form__mail{
  display: none;
  width: 330px;
  margin-top: 5px;
  box-sizing: border-box;
}
.form__comeents{
  margin-top: 5px;
  height: 60px;
  width: 330px;
  box-sizing: border-box;
}
.form__btn1{
  display: none;
  max-width: 80px;
  width: 100%;
}
.form__btn2{
  display: none;
  max-width: 80px;
  width: 100%;
}
.active{
  display: block;
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
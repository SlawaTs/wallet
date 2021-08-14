<template>
  <div class="card">
    <div class="wallet">
      <div class="wallet-balance">
        <div class="wallet-balance__name">{{item.name}}</div>
        <div class="wallet-balance__amount">
          {{currentWallet?.balance ?? '!!!'}}
        </div>
      </div>
      <div class="wallet-control">
        <input type="button" value="Ввод" class="btn wallet-control__in"
               @click="onToggleIn" >
        <input type="button" value="Вывод" class="btn wallet-control__out"
               @click="onToggleOut" >
      </div>
    </div>
    <form class="form" @submit.prevent="onSubmitIn" v-if="formInOpen">
      <input type="text" class="form__sum" placeholder="Сумма"
             v-model="isInput"
             @input="v$.isInput.$touch()">
      <span>Комиссия: {{item.commission}}</span>
      <br>
      <span class="form__valid">{{isValid}}</span>
      <input type="text" class="form__mail" placeholder="Адрес"
             v-if="item.type === 'Криптовалюта'"
             v-model="requisites"
             @input="v$.requisites.$touch()">
      <input type="text" class="form__mail" placeholder="Реквизиты"
             v-if="item.type === 'Фиатная'"
             v-model="requisites"
             @input="v$.requisites.$touch()">

      <textarea class="form__comeents" placeholder="Комментарий"></textarea>
      <div>
        <button class="form__btn1"
                type="submit"
                :class="{active: item.activeIn}"
                :disabled="v$.$invalid">
          Ввод
        </button>
      </div>
    </form>

    <form class="form" @submit.prevent="onSubmitOut" v-else-if="formOutOpen" >
      <input type="text" class="form__sum" placeholder="Сумма"
             v-model="isInput"
             @input="v$.isInput.$touch()">
      <span>Комиссия: {{item.commission}}</span>
      <br>
      <span class="form__valid">{{isValid}}</span>
      <input type="text" class="form__mail" placeholder="Адрес"
             v-if="item.type === 'Криптовалюта'"
             v-model="requisites"
             @input="v$.requisites.$touch()">
      <input type="text" class="form__mail" placeholder="Реквизиты"
             v-if="item.type === 'Фиатная'"
             v-model="requisites"
             @input="v$.requisites.$touch()">

      <textarea class="form__comeents" placeholder="Комментарий"></textarea>
      <div>
        <button class="form__btn2"
                type="submit"
                :class="{active: item.activeOut}"
                :disabled="v$.$invalid">
          Вывод
        </button>
      </div>
    </form>
  </div>
</template>

<script>
import useVuelidate from "@vuelidate/core";
import {required} from "@vuelidate/validators";

export default {
  name: "Card",
  props: {
    user: {
      type: Object,
      required: true,
    },
    item: {
      type: Object,
      required: true,
    }
  },

  setup () {
    return { v$: useVuelidate() }
  },
  data(){
    return {
      requisites: '',
      isInput: '',
      isValid: '',
      formInOpen: false,
      formOutOpen: false,
    }
  },
  validations: {
    requisites:{required,alpha: val =>  /^[0-9]*$/i.test(val)},
    isInput:{required,alpha: val => /^([0-9]*[.]?[0-9]+)$/i.test(val)}
  },

  computed: {
    currentWallet() {
      const currentWallet = this.user.wallets.find(w => this.item.name === w.type);
      return currentWallet;
    }
  },
  methods:{
    onToggleIn() {
      this.formInOpen = !this.formInOpen;
      this.formOutOpen = false;
    },
    onToggleOut() {
      this.formOutOpen = !this.formOutOpen;
      this.formInOpen = false;
    },

    clearInput() {
      this.isInput = '';
      this.isValid = '';
      this.requisites = '';
    },
    onSubmitIn() {
      console.log('onSubmitIn')
      if (this.currentWallet) {
        if (this.item.flagCommis) {
          this.currentWallet.balance += (parseFloat(this.isInput) - (parseFloat(this.isInput) * (parseFloat(this.item.commission))/100));
        } else {
          this.currentWallet.balance += parseFloat(this.isInput) - parseFloat(this.item.commission);
        }
        this.clearInput()
      }
    },
    onSubmitOut() {
      if (this.item.flagCommis) {
        const balanceResult = this.currentWallet.balance - (parseFloat(this.isInput) + (parseFloat(this.isInput) * parseFloat(this.item.commission)/100));
        if (balanceResult) {
          this.currentWallet.balance = balanceResult;
        } else {
          this.isValid = 'Введенное значение меньше вашего баланса!'
        }
      } else {
        const balanceResult = this.currentWallet.balance - (parseFloat(this.isInput) + parseFloat(this.item.commission));
        if (balanceResult) {
          this.currentWallet.balance = balanceResult;
        } else {
          this.isValid = 'Введенное значение меньше вашего баланса!'
        }
      }
      this.clearInput()
    }
  },
}
</script>

<style scoped>

</style>

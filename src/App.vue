<template>
  <h1>CRYPTO1</h1>
  <Input :convert="convert" :change-amount="changeAmount"></Input>
  <p v-if="error !== ''">{{ error }}</p>
  <p v-if="result !== 0" class="result-text">{{ result }}</p>
  <div class="selectors">
    <Selector :setCrypto="setCryptoFirst"></Selector>
    <Selector :setCrypto="setCryptoSecond"></Selector>
  </div>
</template>

<script>
import Input from "@/components/Input.vue";
import Selector from "@/components/Selector.vue";
import CryptoConvert from 'crypto-convert';

const convert = new CryptoConvert(/*options?*/);

export default {
  components: {Input, Selector},
  data() {
    return {
      amount: 0,
      cryptoFirst: '',
      cryptoSecond: '',
      error: '',
      result: 0
    }
  },
  methods: {
    changeAmount(val) {
      this.amount = val
    },
    setCryptoFirst(val) {
      this.cryptoFirst = val
    },
    setCryptoSecond(val) {
      this.cryptoSecond = val
    },
    async convert() {
      if (this.amount <= 0) {
        this.error = "Сумма должна быть больше нуля";
        return;
      } else if (this.cryptoFirst === '' || this.cryptoSecond === '') {
        this.error = 'Выберите валюту';
        return;
      } else if (this.cryptoFirst === this.cryptoSecond) {
        this.error = 'Нельзя выбрать одинаковые валюты';
        return;
      }
      this.error = '';

      await convert.ready();
      try {
        await convert.ready();

        const currencyPairs = {
          'BTC_ETH': convert.BTC.ETH,
          'BTC_USDT': convert.BTC.USD,
          'ETH_BTC': convert.ETH.BTC,
          'ETH_USDT': convert.ETH.USD,
          'USDT_BTC': convert.USD.BTC,
          'USDT_ETH': convert.USD.ETH,
          // Добавьте другие валютные пары по аналогии
        };

        const pairKey = `${this.cryptoFirst}_${this.cryptoSecond}`;

        if (currencyPairs[pairKey]) {
          this.result = currencyPairs[pairKey](this.amount);
        } else {
          this.error = 'Неподдерживаемая валютная пара';
        }

      } catch (error) {
        console.error('Ошибка при конвертации:', error.message);
      }

    }
  }
}

</script>

<style scoped>
.selectors {
  display: flex;
  justify-content: space-around;
  width: 700px;
  margin: 0 auto;
}
</style>

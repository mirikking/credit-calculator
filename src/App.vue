<template>
  <body>
    <main class="calculator-content">
      <div v-if="this.calcLoaded" class="credit-calculator">
        <div class="credit-target">
          <h1 class="purpose-text">Цель кредита</h1>
          <UiSelect 
          :credit_targets="this.creditResponse.credit_target" 
          @selectedOption="getOption"
          ></UiSelect>
        </div>
        <div class="credit-salary-card">
          <p>Наличие зарплатой карты</p>
          <UiCheckbox
          @haveCard="haveCard"
          >Есть/Нет</UiCheckbox>
        </div>
        <div class="credit-housing-price">
          <h1 class="purpose-text">Цена цели кредита</h1>
          <UiInput
          :minSum="this.creditResponse.target_price.min"
          :maxSum="this.creditResponse.target_price.max"
          :step="this.creditResponse.target_price.step"
          @getPrice="getPrice"
          >₽</UiInput>
        </div>
        <div class="credit-initional-payment">
          <h1 class="purpose-text">Первоначальный взнос</h1>
          <UiInput
          :minSum="this.creditResponse.init_payment.min"
          :maxSum="this.creditResponse.init_payment.max"
          :step="this.creditResponse.init_payment.step"
          @getInitPayment="getInitPayment"
          >₽</UiInput>
        </div>
        <div class="credit-period">
          <h1 class="purpose-text">Срок кредита</h1>
          <UiInput
          :minSum="this.creditResponse.credit_period.min"
          :maxSum="this.creditResponse.credit_period.max"
          :step="this.creditResponse.credit_period.step"
          @getDuration="getDuration"
          >мес.
          </UiInput>
        </div>
        <div class="credit-proceed">
          <UiButton
          @click="openTable"
          >Продолжить</UiButton>
        </div>
      </div>
      <UiTable 
      v-if="this.isProceed"
      :isHaveSalaryCard="this.isHaveSalaryCard"
      :targetPrice="this.targetPrice"
      :initialPayment="this.initialPayment"
      :creditPeriod="this.creditPeriod"
      :currentMultiplier="this.currentMultiplier"
      @closeTable="closeTable"
      ></UiTable>
      <div v-if="!this.calcLoaded">
        <div class="gg-spinner">
        </div>
      </div>
    </main>
  </body>
</template>
<script lang="ts">
export default {
  data() {
    return {
      calcLoaded: false as Boolean, // default // избежать ошибок до рендера
      creditResponse: {} as Object,
      currentMultiplier: 1 as Number, // as default multiplier
      isProceed: false as Boolean,
      isHaveSalaryCard: false as Boolean, // default
      targetPrice: 1000000 as Number, // default
      initialPayment: 100000 as Number, 
      creditPeriod: 10 as Number,
      tempMultiplier: 1 as Number,
    }
  },
  mounted() {
    fetch("https://630aa526f280658a59d0e2de.mockapi.io/api/goods/credit")
    .then(response => response.json())
    .then((result: Array<Object>) => {
      this.creditResponse = result[0];
      this.calcLoaded = true
    })
  },
  methods: {
    openTable() {
      if (Number(this.initialPayment) > Number(this.targetPrice)) {
        alert("Первоначальный взнос не может быть больше суммы кредита")
      } else if (this.currentMultiplier == 0.8 || this.currentMultiplier == 1) {
        alert("Выберите цель кредита")
      } else {
        this.isProceed = true;
      }
    },
    haveCard(checked: Boolean) { 
      if (checked) {
        this.currentMultiplier = this.currentMultiplier * this.creditResponse.card_multiplier
      } else {
        this.currentMultiplier = this.tempMultiplier
      }
    },
    getPrice(sum: Number) {
      this.targetPrice = sum;
    },
    getInitPayment(initPayment: Number) {
      this.initialPayment = initPayment;
    },
    getDuration(duration: Number) {
      this.creditPeriod = duration
    },
    getOption(option: Number) {
        this.currentMultiplier = option
        this.tempMultiplier = option
    },
    closeTable(){
      this.isProceed = false
    }
  }
}
</script>
<style>
@import "@/assets/main.css";

.calculator-content {
  width: 100%;
  height: 100vh;
  align-items: center;
  justify-content: center;
  display: flex;
  gap: 42px;
}

.purpose-text {
  color: white;
}

.credit-calculator {
  display: flex;
  flex-direction: column;
  gap: 36px;
  animation-name: appearance;
  animation-duration: 1.5s;
  animation-iteration-count: 1;
}

.credit-target,
.credit-housing-price,
.credit-initional-payment,
.credit-period {
  display: flex;
  flex-direction: column;
  gap: 12px;
  align-items: center;
}

.credit-salary-card {
  display: flex;
  flex-direction: row;
  color: white;
  align-items: center;
  justify-content: space-between;
}

.gg-spinner {
    transform: scale(var(--ggs,1))
}
.gg-spinner,
.gg-spinner::after,
.gg-spinner::before {
    box-sizing: border-box;
    position: relative;
    display: block;
    width: 40px;
    height: 40px
}
.gg-spinner::after,
.gg-spinner::before {
    content: "";
    position: absolute;
    border-radius: 100px
}
.gg-spinner::before {
    animation: spinner 1s
    cubic-bezier(.6,0,.4,1) infinite;
    border: 3px solid white;
    border-top-color: currentColor
}
.gg-spinner::after {
    border: 3px solid;
    opacity: .2
}
@keyframes spinner {
    0% { transform: rotate(0deg) }
    to { transform: rotate(359deg) }
}

@keyframes appearance {
    from {
      opacity: 0;
    }
    to {
      opacity: 1;
    }
}


</style>
<template>
    <div class="credit-modal">
      <div class="credit-table">
        <button class="credit-table-close" @click="this.$emit('closeTable', true)">x</button>
        <table width="100%" class="payment-schedule">
        <caption class="table-header"><h1>График платежей</h1></caption>
          <tr class="table-column">
            <th>Месяц/год</th>
            <th>Сумма платежа</th>
            <th>Основной долг</th>
            <th>Проценты</th>
            <th>Остаток</th>
          </tr>
          <tr align="center" class="table-payment-info" v-for="payment in this.payments">
            <td> {{ payment.date }} </td>
            <td> {{ Math.round(payment.payment.currentPay) }} </td>
            <td> {{ Math.round(payment.payment.debtPercents) }} </td>
            <td> {{ Math.round(payment.payment.mainDebt) }} </td> 
            <td> {{ Math.round(payment.payment.remaining) }}  </td>
          </tr>
        </table>
      </div>
    </div>
</template>
<script lang="ts">
export default {
    name: "UiTable",
    data() { 
      return {
        payments: [] as Array<[]>,
        currentMonth: null  as unknown as number,
        currentYear: null as unknown as number,
        remainingDebt: 0 as number,
      }
    },
    props: {
      isHaveSalaryCard: Boolean,
      targetPrice: Number,
      initialPayment: Number,
      creditPeriod: Number,
      currentMultiplier: Number,
    },
    mounted() {
      let currentCreditSumOverpayed = ((Number(this.targetPrice) - Number(this.initialPayment)) * (Number(this.currentMultiplier) * 0.01) + (Number(this.targetPrice) - Number(this.initialPayment))) * this.currentMultiplier
      let currentCreditSum = Number(this.targetPrice) - Number(this.initialPayment)
      let overpayedSum = currentCreditSumOverpayed - currentCreditSum
      const date = new Date()
      this.currentMonth = date.getMonth()
      this.currentYear = date.getFullYear()
      this.remainingDebt = currentCreditSumOverpayed 
      for (let i = 0; i < this.creditPeriod; i ++) {
        this.payments.push({
          "date": this.setCreditDate(i),
          "payment": this.setPayment(currentCreditSumOverpayed, overpayedSum, this.creditPeriod, this.remainingDebt)
        })
      }
    },
    methods: {
      setCreditDate(i: number) { 
        if (this.currentMonth + 1 > 12) {
          this.currentYear += 1
          this.currentMonth = 1
          return `${this.currentMonth}/${this.currentYear}`
        } else {
          this.currentMonth += 1
          return `${this.currentMonth}/${this.currentYear}` 
        }
      },
      setPayment(currentCreditSumOverpayed: number, overpayedSum: number, creditPeriod: number, remainingDebt: number) {
        let currentPay = currentCreditSumOverpayed / creditPeriod;
        let mainDebt = (currentCreditSumOverpayed - overpayedSum) / creditPeriod; 
        let debtPercents = overpayedSum / creditPeriod;
        this.remainingDebt -= currentPay;
        return {"currentPay": currentPay, "mainDebt": mainDebt, "debtPercents": debtPercents, "remaining": this.remainingDebt}
      }
    }
    
}
</script>
<style scoped>

.payment-schedule {
  height: 100%;
}

.credit-modal {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
  height: 100vh;
  position: fixed;
  top: 0;
  background: rgba(0, 0, 0, 0.301);
  z-index: 999;
}

.credit-table {
  width: 80%;
  background: white;
  height: 80vh;
  border-radius: 32px;
  box-sizing: border-box;
  padding: 2%;
  overflow-y: auto;
}

.credit-table::-webkit-scrollbar {
  width: 5px;
  height: 8px;
  background-color: none;
}

.credit-table::-webkit-scrollbar-thumb {
  background: rgb(56, 56, 56);
  border-radius: 2px;
}

.credit-table::-webkit-scrollbar-track-piece:end {
    background: transparent;
    margin-bottom: 20px; 
}

.credit-table::-webkit-scrollbar-track-piece:start {
    background: transparent;
    margin-top: 20px;
}


.table-header {
  margin-bottom: 24px;
}

.table-payment-info {
  margin-top: 12px;
}

.credit-table-close {
  width: 20px;
  height: 20px;
  border: none;
  border-radius: 5px;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  box-sizing: border-box;
  padding-bottom: 1px;
  color: black;
  position: absolute;
  transition: all 0.2s;
}

.credit-table-close:hover {
  -webkit-box-shadow: 0px 0px 3px 0px rgba(34, 60, 80, 0.2);
  -moz-box-shadow: 0px 0px 3px 0px rgba(34, 60, 80, 0.2);
  box-shadow: 0px 0px 3px 0px rgba(34, 60, 80, 0.2);
}
    
</style>
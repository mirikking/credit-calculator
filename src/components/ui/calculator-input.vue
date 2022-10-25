<template>
    <div class="ui-inputs">
        <div class="ui-input-content">
            <input
                class="ui-input-number" 
                type="number" 
                v-model="this.currentSum" 
                :min="this.minSum" 
                :max="this.maxSum" 
                @input="fillBg(); changedPrice()" 
                @click="changeBg" 
                @focusout="changeBg"
                @change="limit"
            >
            <h2 class="ui-input-additional">
            <slot></slot>
            </h2>
        </div>
        <input 
            class="ui-input-range" 
            type="range" 
            v-model="this.currentSum" 
            :step="this.step" 
            :min="this.minSum" 
            :max="this.maxSum" 
            @input="fillBg(); changedPrice()"
        >
    </div>
</template>
<script lang="ts">
export default {
    name: "UiInput",
    data() {
        return {
            currentSum: this.minSum as Number,
        }
    },
    props: {
        minSum: Number,
        maxSum: Number,
        step: Number
    }, 
    methods: {
        fillBg() {
            let percentage = (this.currentSum - this.minSum) / (this.maxSum - this.minSum) * 100;
            this.$el.querySelector('.ui-input-range').style = 'background: linear-gradient(to right, #FF9514, #FF9514 ' + percentage + '%, #E1E1E1 ' + percentage + '%, #E1E1E1 100%)';
        },
        limit() {
            if (this.currentSum > this.maxSum) {
                this.currentSum = this.maxSum
            }
            if (this.currentSum < this.minSum) {
                this.currentSum = this.minSum
            }
        },
        changedPrice() {
            this.$emit("getPrice", this.currentSum)
            this.$emit("getPercent", this.currentSum)
            this.$emit("getDuration", this.currentSum)
            this.$emit("getInitPayment", this.currentSum)
        },
        changeBg() {
            if (!this.$el.classList.contains("active")) {
                this.$el.classList.add("active")
            } else {
                this.$el.classList.remove("active")
            }
        }
    }
}
</script>
<style scoped>
.ui-inputs {
  display: flex;
  flex-direction: column;
  background: #F3F3F4;
  border-radius: 16px;
  width: 427px;
  height: 68px;
  justify-content: space-between;
  box-sizing: border-box;
  padding: 16px 24px 0px 24px;
}
.ui-inputs.disabled {
  opacity: 0.4;
}
.ui-inputs.active {
  background: white;
  border: 2px solid #F3F3F4;
  padding: 14px 22px 0px 22px;
}
/* number */
.ui-input-content {
  display: flex;
  flex-direction: row;
  margin: 0;
  padding: 0;
  height: 38px;
}
.ui-input-number {
  background-color: transparent;
  border: none;
  outline: 0;
  font-style: normal;
  font-weight: 900;
  font-size: 30px;
  height: 36px;
  color: #575757;
  width: 90%;
  -moz-appearance: textfield;
}
.ui-input-additional {
  margin: 0;
  padding: 0;
  margin-left: auto;
  font-style: normal;
  font-weight: 900;
  font-size: 30px;
  color: #575757;
  user-select: none;
  height: 36px;
  display: flex;
  align-items: center;
}
.ui-input-number::-webkit-inner-spin-button {
  display: none;
}
/* end */
/* range */
.ui-input-range-bg {
  width: 100%;
  height: auto;
  background: #FF9514;
  height: 2px;
}
.ui-input-range {
  appearance: none;
  cursor: pointer;
  margin: 0;
  width: 100%;
  height: 2px;
}
.ui-input-range::-webkit-slider-runnable-track {
  height: 2px;
  background: transparent;
  z-index: 9;
}
.ui-input-range::-moz-range-track {
  height: 4px;
  background: transparent;
  z-index: 9;
}
.ui-input-range::-webkit-slider-thumb {
  margin-top: -10px;
  width: 20px;
  height: 20px;
  -webkit-appearance: none;
  appearance: none;
  background: #FF9514;
  border-radius: 100%;
}
.ui-input-range::-moz-range-thumb {
  margin-top: -16px; 
  width: 20px;
  height: 20px;
  -webkit-appearance: none;
  appearance: none;
  background: #FF9514;
  border-radius: 100%;
  border: none;
}
.ui-input-range:disabled {
  cursor: default;
}
.ui-inputs.active > .ui-input-range {
  margin-top: 14px;
}
/* end */

@media (max-width: 319px) {
  .ui-input-number {
    font-weight: 900;
    font-size: 22px;
    line-height: 20px;
  }
  .ui-input-additional {
    font-weight: 900;
    font-size: 22px;
    line-height: 20px;
  }
}

</style>
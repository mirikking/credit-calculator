<template>
    <select v-model="this.selectedOption" class="ui-target-select">
        <option value="null" selected hidden>Выберете цель кредита</option>
        <option v-for="target in this.credit_targets" :value="target.multiplier" :key="target.id">{{ target.name }}</option>
    </select>
</template>
<script lang="ts">
export default {
    name: "UiSelect",
    props: {
        credit_targets: {
            type: Array,
            default: [] 
        }
    },
    data() {
        return {
            selectedOption: null as unknown as Number
        }
    },
    watch: {
        selectedOption() {
            this.$emit("selectedOption", this.selectedOption);
        }
    }
}
</script>
<style scoped>

.ui-target-select {
    width: 427px;
    height: 68px;
    background: white;
    border: none;
    color: #575757;
    border-radius: 16px;
    display: flex;
    align-items: center;
    justify-content: center;
    font-weight: 900;
    font-size: 22px;
    outline: 0;
    box-sizing: border-box;
    padding: 2%;
}

</style>
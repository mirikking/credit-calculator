<template>
    <div class="ui-target-salary-card">
        <label for="Salary Card">
            <slot></slot>
        </label>
        <input class="ui-checkbox" type="checkbox" name="Salary Card" value="true" v-model="this.haveSalaryCard" @change="isHaveCard"/>
    </div>
</template>
<script lang="ts">
export default {
    name: "UiCheckbox",
    data() {
        return {
            haveSalaryCard: Boolean
        }
    },
    methods: {
        isHaveCard(event: Event) {
            this.$emit("haveCard", this.haveSalaryCard)
        }
    }
}
</script>
<style scoped>

.ui-target-salary-card {
    display: flex;
    flex-direction: row;
    gap: 10px;
    align-items: center;
}

.ui-checkbox {
    width: 25px;
    height: 25px;
    accent-color: black;
}

    
</style>
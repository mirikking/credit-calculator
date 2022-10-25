<template>
    <el-select v-model="this.currentValue" filterable placeholder="Выберите ставку">
        <el-option
        v-for="item in options"
        :key="item"
        :label="item"
        :value="item"
        />
    </el-select>
</template>
<script lang="ts">
export default {
    name: 'UiCretidTarget',
    data() {
        return {
            currentValue: null
        }
    },
    props: {
        options: {
            type: Array
        }
    },
}
</script>
<style>
    
</style>
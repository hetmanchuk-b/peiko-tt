<template>
    <div class="app__table" v-if="listItems">
        <div class="app__table-row --head">
            <span class="app__table-cell">Stock</span>
            <span class="app__table-cell">Current</span>
            <span class="app__table-cell">Change</span>
        </div>
        <div class="app__table-row"
             v-for="stock in listItems"
             :key="stock.company">
            <span class="app__table-cell">
                {{ stock.company }}
            </span>
            <span class="app__table-cell">
                {{ stock.current }}
            </span>
            <span class="app__table-cell" :class="getClassByDifference(stock.current, stock.start)">
                {{ getDifference(stock.current, stock.start) }}
            </span>
        </div>
    </div>
</template>

<script>
export default {
    name: 'DataTable',
    props: {
        listItems: {
            type: Array
        }
    },
    methods: {
        getDifference(current, start) {
            const result = (current - start).toFixed(2);
            if (result >= 0) {
                return `+${result.toString()}`;
            } else {
                return result;
            }
        },
        getClassByDifference(current, start) {
            if (current - start >= 0) {
                return 'green';
            } else {
                return 'red';
            }
        }
    }
}
</script>

<style lang="scss"></style>

<template>
    <div id="app">
        <img alt="Vue logo" src="./assets/peiko-logo-mini.png">
        <div class="app__wrapper">
            <div class="app__wrapper-btn">
                <data-button :handler="getData" />
            </div>

            <loader :show="loading"/>

            <data-table v-if="rawData && !loading && !error" :list-items="stockData" />

            <div class="nothing" v-if="error && !loading"><strong>{{ error }}</strong></div>
        </div>
        <!-- /.app__wrapper -->
    </div>
</template>

<script>

import simulateAsyncReq from "./plugins/getDataFunc";
import { payload } from "./mocData";
import Loader from "./components/Loader";
import DataButton from "./components/DataButton";
import DataTable from "./components/DataTable";

export default {
    name: 'App',
    components: { DataTable, DataButton, Loader },
    data() {
        return {
            rawData: null,
            loading: false,
            error: ''
        }
    },
    created() {
    },
    methods: {
        getData() {
            this.loading = true;
            simulateAsyncReq(payload)
                .then(response => {
                    this.rawData = response;
                    this.error = '';
                    this.loading = false;
                })
                .catch(error => {
                    console.error(error);
                    this.error = 'No data.';
                    this.loading = false;
                });
        }
    },
    computed: {
        stockData() {
            const result = [];
            this.rawData.stocks.forEach((stock, index) => {
                result.push({
                    company: this.rawData.stocks[index],
                    start: Number([this.rawData.start[index]]).toFixed(2),
                    current: Number([this.rawData.current[index]]).toFixed(2)
                });
            });
            return result.sort((a, b) => {
                if (a.company > b.company) {
                    return 1;
                } else if (a.company < b.company) {
                    return -1;
                } else {
                    return 0;
                }
            });
        }
    }
}
</script>

<style lang="scss">
    $fastTransition: .13s ease-in-out;
    $grey: #f5f5f5;

    body {
        margin: 0;
        font-size: 16px;
    }
    *, *::after, *::before {
        box-sizing: border-box;
    }
    #app {
        font-family: Avenir, Helvetica, Arial, sans-serif;
        -webkit-font-smoothing: antialiased;
        -moz-osx-font-smoothing: grayscale;
        text-align: center;
        color: #2c3e50;
        margin-top: 60px;
    }
    .nothing {
        font-size: 18px;
        text-transform: uppercase;
        font-weight: 700;
        color: maroon;
    }
    .btn {
        outline: none;
        display: block;
        padding: 6px 36px;
        border-radius: 5px;
        border: 1px solid #2c3e50;
        background-color: #ffffff;
        cursor: pointer;
        font-size: 16px;
        font-weight: 600;
        transition: background-color $fastTransition;
        &:hover {
            background-color: $grey;
        }
    }
    .app {
        &__wrapper {
            display: flex;
            flex-direction: column;
            align-items: center;
            &-btn {
                margin-bottom: 1.5rem;
            }
        }
        &__table {
            border-radius: 5px;
            border: 1px solid $grey;
            width: 100%;
            max-width: 420px;
            &-row {
                display: flex;
                justify-content: space-between;
                align-items: center;
                min-height: 42px;
                border-bottom: 1px solid $grey;
                padding: 4px 15px;
                background-color: #ffffff;
                transition: background-color $fastTransition;
                &:hover {
                    background-color: rgba($grey, .4);
                }
                &.--head {
                    background-color: $grey;
                    font-weight: 700;
                }
                &:last-child {
                    border-bottom: 0;
                }
            }
            &-cell {
                display: block;
                overflow: hidden;
                width: 33.333%;
                text-align: center;
                &.red {
                    color: red;
                }
                &.green {
                    color: forestgreen;
                }
                &:first-child {
                    text-align: left;
                }
                &:last-child {
                    text-align: right;
                }
            }
        }
    }
</style>

<template>
    <div>
        <b-button @click="modalVisible = !modalVisible" variant="secondary">History</b-button>
        <b-modal v-model="modalVisible" @shown="toggleChart" @hidden="toggleChart" title="Day Length" size="xl" :hide-footer="true">
            <div  style="height:400px;">
                <transition name="fade">
                    <ChartContainer class="chartContainer"
                                    v-if="chartVisible"
                                    :location="location" :start-date="new Date(startDate)"
                                    :end-date="new Date(endDate)"></ChartContainer>
                </transition>
            </div>
            <b-form class="mt-2">
                <b-input-group inline>
                    <b-form-datepicker v-model="startDate" id="startDate" class="mb-2"></b-form-datepicker>
                    <b-form-datepicker v-model="endDate" id="endDate" class="mb-2"></b-form-datepicker>
                </b-input-group>
            </b-form>
        </b-modal>
    </div>
</template>

<script>
    import ChartContainer from "./ChartContainer";

    export default {
        name: "History",
        components: {ChartContainer},
        data: () => {
            return {
                startDate: new Date(new Date().getTime() - (60 * 24 * 60 * 60 * 1000)),
                endDate: new Date(new Date().getTime() + (60 * 24 * 60 * 60 * 1000)),
                modalVisible: false,
                chartVisible: false,
            }
        },
        props: {
            location: Object
        },
        methods: {
            toggleChart() {
                this.chartVisible = !this.chartVisible
            }
        }
    }
</script>

<style scoped>
    .fade-enter-active, .fade-leave-active {
        transition: opacity .5s;
    }

    .fade-enter, .fade-leave-to /* .fade-leave-active below version 2.1.8 */
    {
        opacity: 0;
    }
</style>
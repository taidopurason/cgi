<template>
    <LineChart
            :chartdata="chartdata"
            :options="options" v-if="this.renderComponent"/>
</template>

<script>
    import LineChart from "./LineChart";
    import SunCalc from 'suncalc';

    export default {
        name: 'ChartContainer',
        components: {LineChart},
        props: {
            startDate: Date,
            endDate: Date,
            location: Object
        },
        data: () => ({
            chartdata: {
                labels: [],
                datasets: [
                    {
                        label: 'Day Length',
                        backgroundColor: '#ffff',
                        data: [],
                    }
                ]
            },
            options: {
                responsive: true,
                maintainAspectRatio: false,
                legend: {display : false},
            },
            renderComponent: true
        }),
        methods: {
            calculateDayLength(date){
                const times = SunCalc.getTimes(date, this.location.lat, this.location.long);
                return Math.abs(times.sunrise - times.sunset)
            },
            calculateData(){
                this.chartdata.labels = [];
                this.chartdata.datasets[0].data = [];
                for (let date = new Date(this.startDate); date <= this.endDate; date.setDate(date.getDate() + 1)) {
                    this.chartdata.labels.push(date.getFullYear() + "/" + (date.getMonth() + 1) + "/" + date.getDate());
                    this.chartdata.datasets[0].data.push(this.calculateDayLength(date))
                }
                this.forceRerender();
            },
            forceRerender() {
                this.renderComponent = false;
                this.$nextTick(() => {
                    this.renderComponent = true;
                });
            }
        },
        watch: {
            location: {
                handler(){
                    this.calculateData()
                },
                deep: true
            }
        }
    }
</script>

<style scoped>

</style>
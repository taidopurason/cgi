<template>
    <div>
        <LineChart :height="400"
                :chartdata="chartdata"
                :options="options" v-if="this.renderComponent"/>
    </div>
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
                legend: {display: false},
                scales: {
                    yAxes: [{
                        scaleLabel: {
                            display: true,
                            labelString: 'minutes'
                        }
                    }]
                }
            },
            renderComponent: true
        }),
        methods: {
            calculateDayLength(date) {
                const times = SunCalc.getTimes(date, this.location.lat, this.location.long);
                if (times.sunset instanceof Date && !isNaN(times.sunset)) {
                    return Math.floor(Math.abs(times.sunrise - times.sunset) / (1000 * 60))
                } else {
                    if (SunCalc.getPosition(date, this.location.lat, this.location.long).altitude > 0) {
                        return 60 * 24
                    } else {
                        return 0
                    }
                }
            },
            calculateData() {
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
        mounted(){
            this.calculateData()
        },
        watch: {
            location: {
                handler() {
                    this.calculateData()
                },
                deep: true
            },
            startDate: function() {
                this.calculateData();
            },
            endDate: function() {
                this.calculateData();
            }
        }
    }
</script>

<style scoped>

</style>
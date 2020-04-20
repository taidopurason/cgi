<template>
    <div class="container">
        <div class="row">
            <div class="col-sm">
                <Form :submit="calculateTime"></Form>
            </div>
        </div>
        <div class="row">
            <div class="col-sm">
                <h2>Sunrise time</h2><br>
                <h2>{{sunriseTime}}</h2>
            </div>
            <div class="col-sm">
                <h2>Sunset time</h2><br>
                <h2>{{sunsetTime}}</h2>
            </div>
            <div class="col-sm">
                <h2>Day length</h2><br>
                <h2>{{dayLength}}</h2>
            </div>
        </div>
    </div>
</template>

<script>
    import SunCalc from 'suncalc';
    import Form from "./Form";

    export default {
        name: "DayLength",
        components: {Form},
        data: () => {
            return {
                sunriseTime: "placeholder",
                sunsetTime: "placeholder",
                dayLength: "placeholder"
            }
        },
        methods: {
            calculateTime(lat, long, date) {
                window.console.log(lat, long, date);
                const times = SunCalc.getTimes(date, lat, long);

                this.sunriseTime = this.formatTimeToString(times.sunrise.getHours(), times.sunrise.getMinutes());
                this.sunsetTime = this.formatTimeToString(times.sunset.getHours(),times.sunset.getMinutes());

                const diffTime = Math.abs(times.sunrise - times.sunset);
                const hours = Math.floor(diffTime / (1000 * 60 * 60));
                const minutes = Math.floor(diffTime / (1000 * 60) % 60);
                this.dayLength = hours + " h " + minutes + " mins";
            },
            formatTimeToString(hours, mins){
                hours = hours.toString();
                mins = mins.toString();
                if (hours.length === 1)
                    hours = '0' + hours;
                if (mins.length === 1)
                    mins = '0' + mins;
                return hours + ":" + mins
            }
        }
    }

</script>

<style scoped>

</style>
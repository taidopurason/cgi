<template>
    <div class="container">
        <div class="row">
            <div class="col-sm">
                <Form :submit="updateLocation"></Form>
            </div>
        </div>
        <div class="row">
            <div class="col-sm">
                <h2>Sunrise time</h2>
                <div class="time">{{sunriseTime}}</div>
            </div>
            <div class="col-sm">
                <h2>Sunset time</h2>
                <div class="time">{{sunsetTime}}</div>
            </div>
            <div class="col-sm">
                <h2>Day length</h2>
                <div class="time">{{dayLength}}</div>
            </div>
        </div>
        <Map :location="location" :change-location="updateLocation"></Map>
    </div>
</template>

<script>
    import SunCalc from 'suncalc';
    import Form from "./Form";
    import Map from "./Map";

    export default {
        name: "DayLength",
        components: {Map, Form},
        data: () => {
            return {
                location: {lat: 58.378025, long: 26.728493},
                date: new Date(),
                sunriseTime: "",
                sunsetTime: "",
                dayLength: ""
            }
        },
        methods: {
            updateLocation(location, date = this.date){
                this.calculateTime(location, date);
                this.date = date;
                this.location = location
            },
            calculateTime(location, date) {
                const times = SunCalc.getTimes(date, location.lat, location.long);

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
        },
        created() {
            this.calculateTime(this.location, new Date())

        }
    }

</script>

<style scoped>
    .time{
        font-weight: bold;
        font-size: 2em;
    }

</style>
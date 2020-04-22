<template>
    <div class="container">
        <b-row class="mt-2">
            <b-col>
                <h3>Sunrise time</h3>
                <div class="time">{{sunriseTime}}</div>
                <div>UTC</div>
            </b-col>
            <b-col>
                <h3>Day length</h3>
                <div class="time">{{dayLength}}</div>
            </b-col>
            <b-col>
                <h3>Sunset time</h3>
                <div class="time">{{sunsetTime}}</div>
                <div>UTC</div>
            </b-col>
        </b-row>

        <b-row class="mt-2">
            <b-col>
                <History :location="location"></History>
            </b-col>
        </b-row>

        <b-row class="mt-5">
            <b-col>
                <Form :submit="updateLocation" :current-location="location"></Form>
            </b-col>
        </b-row>

        <b-row class="mt-2">
            <b-col>
                <Map :location="location" :change-location="updateLocation"></Map>
            </b-col>
        </b-row>

    </div>
</template>

<script>
    import SunCalc from 'suncalc';
    import Form from "./Form";
    import Map from "./Map";
    import History from "./History";

    export default {
        name: "DayLength",
        components: {History, Map, Form},
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
            updateLocation(location, date = this.date) {
                this.calculateTime(location, date);
                this.date = date;
                this.location = location
            },
            calculateTime(location, date) {
                const times = SunCalc.getTimes(date, location.lat, location.long);
                if (times.sunset instanceof Date && !isNaN(times.sunset)) {
                    this.sunriseTime = this.formatTimeToString(times.sunrise.getUTCHours(), times.sunrise.getUTCMinutes());
                    this.sunsetTime = this.formatTimeToString(times.sunset.getUTCHours(), times.sunset.getUTCMinutes());

                    const diffTime = Math.abs(times.sunrise - times.sunset);
                    const hours = Math.floor(diffTime / (1000 * 60 * 60));
                    const minutes = Math.floor(diffTime / (1000 * 60) % 60);
                    this.dayLength = hours + " h " + minutes + (minutes === 1 ? " mins" : " min");
                } else {
                    this.sunriseTime = "--:--";
                    this.sunsetTime = "--:--";
                    if (SunCalc.getPosition(date, location.lat, location.long).altitude > 0) {
                        this.dayLength = "24 h 0 mins"
                    } else {
                        this.dayLength = "0 h 0 mins"
                    }
                }

            },
            formatTimeToString(hours, mins) {
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
    .time {
        font-weight: bold;
        font-size: 2em;
    }


</style>
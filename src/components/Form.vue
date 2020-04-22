<template>
    <b-form>
        <b-input-group inline class="mb-2">
            <b-input alt="latitude" v-model="location.lat"
                    id="latitude"
            ></b-input>
            <b-input alt="longitude" v-model="location.long"
                    id="longitude"
            ></b-input>
        </b-input-group>
        <b-form-datepicker v-model="date" id="date" class="mb-2"></b-form-datepicker>
        <b-button variant="primary" @click="submit({lat: location.lat, long: location.long}, new Date(date))">Update</b-button>
    </b-form>
</template>

<script>

    export default {
        name: "Form",
        props:{
            submit: Function,
            currentLocation: Object
        },
        data: ()  => {
            return {
                date: new Date(),
                location: {lat: 0, long: 0}
            }
        },
        watch: {
            currentLocation: {
                handler(){
                    this.location = {lat: this.currentLocation.lat, long: this.currentLocation.long}
                },
                deep: true
            }
        }
    }
</script>

<style scoped>

</style>
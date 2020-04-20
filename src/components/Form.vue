<template>
    <b-form>
        <b-input-group inline class="mb-2">
            <b-input v-model="latitude"
                    id="latitude"
            ></b-input>
            <b-input v-model="longitude"
                    id="longitude"
            ></b-input>
        </b-input-group>
        <label for="date">Choose a date</label>
        <b-form-datepicker v-model="date" id="date" class="mb-2"></b-form-datepicker>
        <b-button variant="primary" @click="submit(latitude, longitude, new Date(date))">Find</b-button>
    </b-form>
</template>

<script>
    export default {
        name: "Form",
        props:{
            submit: Function
        },
        data: ()  => {
            return {
                date: new Date(),
                latitude: 0,
                longitude: 0
            }
        }
    }
</script>

<style scoped>

</style>
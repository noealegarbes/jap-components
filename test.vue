<template>
    <v-container class="ma-0 d-flex flex-column justify-start w-100">
        
        <!-- sample use for japDatepicker -->
        <v-row class="pa-3 w-75">
            <v-col class="mx-2 pa-0" cols="4">
                <p>Default</p>
                <JapDatePicker 
                    @selected-date="dateValue1 = $event"
                />
                <p>Return Value: {{ dateValue1 }}</p>
            </v-col>
            <v-col class="mx-2 pa-0" cols="4">
                <p>Specific Mode</p>
                <JapDatePicker 
                    @selected-date="dateValue2 = $event"
                    mode="year"
                />
                <p>Return Value: {{ dateValue2 }}</p>
            </v-col>
        </v-row>

    </v-container>
</template>

<script setup>
    import { ref } from "vue";

    const dateValue1 = ref(null)
    const dateValue2 = ref(null)

</script>

<style scoped></style>

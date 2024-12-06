<template>
    <v-menu :close-on-content-click="false">
        <template v-slot:activator="{ props }">
            <v-text-field
                variant="outlined"
                density="compact"
                v-bind="{ ...props, ...$attrs }"
                readonly
                clearable
                v-model="formattedDate"
                @click:clear="clearDate"
                placeholder="Select Date"
                append-inner-icon="mdi-calendar"
            ></v-text-field>
        </template>
        <v-locale-provider
            :locale="DATE_FORMAT_JA_JP"
            :fallback-locale="DATE_FORMAT_JA"
        >
            <v-date-picker
                v-model="selectedDate"
                hide-header
                :view-mode="viewMode"
                elevation="4"
                @update:year="onYearSelected"
                @update:month="onMonthSelected"
            ></v-date-picker>
        </v-locale-provider>
    </v-menu>
</template>

<script setup>
    /**
     * @prop {string} mode = use to change view-mode of date-picker | accepted values: 'month', 'months', 'year'
     *
     * Refer to /test page for demo use
     *
     */
    import { ref, computed, watch } from "vue";
    import { DATE_FORMAT_JA, DATE_FORMAT_JA_JP, YEAR_MONTH_DAY_DASH } from "~/constants/datetime";

    const props = defineProps(["mode", "defaultDate"]);

    const selectedDate = ref(null);
    const viewMode = ref(props.mode || "month");
    const emit = defineEmits(["selectedDate"]);

    const formattedDate = computed(() => {
        if (selectedDate.value) {
            const commonDate = new Intl.DateTimeFormat("ja-JP", {
                year: "numeric",
                month: "2-digit",
                day: "2-digit",
            }).format(new Date(selectedDate.value));

            const displayDate = formatDisplay(commonDate);
            updateDateResults(commonDate);
            return displayDate;
        }
        return "";
    });

    const updateDateResults = (commonDate) => {
        const formattedDate = FormatDate(commonDate, YEAR_MONTH_DAY_DASH);
        emit("selectedDate", !isValidDate(formattedDate) ? null : formattedDate);
    };

    const formatDisplay = (date) => {
        const dateObj = new Date(date);
        const japaneseDateShort = dateObj
            .toLocaleDateString(DATE_FORMAT_JA_JP, { era: "long" })
            .replace(/(\d+)\/(\d+)\/(\d+)/, "$1年");
        return `${japaneseDateShort}(${date})`;
    };

    const clearDate = () => {
        selectedDate.value = null;
        updateDateResults(null, null);
        viewMode.value = "year";
    };

    // Functions to handle view mode change on selection
    const onYearSelected = () => {
        viewMode.value = "months";
    };

    const onMonthSelected = () => {
        viewMode.value = "month";
    };

    onMounted(() => {
        selectedDate.value = props.defaultDate ? new Date(props.defaultDate) : null;
    });

    watch(selectedDate, (newValue) => {
        if (newValue == null) {
            updateDateResults(null, null);
            if(!props.mode) {
                viewMode.value = "month";
            }
        }
    });
</script>

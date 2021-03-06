<template>
    <VueDatepicker
        v-bind="$props"
        v-on="$listeners">
            <slot name="beforeCalendarHeader" slot="beforeCalendarHeader" />
            <slot name="afterDateInput" slot="afterDateInput" />
    </VueDatepicker>
</template>

<script>
import VueDatepicker from 'vuejs-datepicker'

// Validator function that checks the date props.
const _datePropValidator = (v) => {
    return v === null
            || v instanceof Date
            || typeof v === 'string'
            || typeof v === 'number'
}

export default {
    name: 'd-datepicker',
    components: { VueDatepicker },
    props: {
        /**
         * The datepicker's value.
         */
        value: {
            validator: _datePropValidator
        },
        /**
         * The name.
         */
        name: {
            type: String,
            default: null
        },
        /**
         * The component's ID.
         */
        id: {
            type: String,
            default: null
        },
        /**
         * The date format.
         */
        format: {
            type: [String, Function],
            default: 'dd MMM yyyy'
        },
        /**
         * The language.
         */
        language: Object,
        /**
         * If set, the datepicker will open on this date.
         */
        openDate: {
            validator: _datePropValidator
        },
        /**
         * Function used to render custom content inside the day cell.
         */
        dayCellContent: Function,
        /**
         * Whether to show the full month name, or not.
         */
        fullMonthName: Boolean,
        /**
         * Configure disabled dates.
         */
        disabledDates: Object,
        /**
         * Highlight dates.
         */
        highlighted: Object,
        /**
         * The placeholder.
         */
        placeholder: String,
        /**
         * Show the datepicker always open.
         */
        inline: Boolean,
        /**
         * The CSS class applied to the calendar element.
         */
        calendarClass: [String, Object, Array],
        /**
         * The CSS Class applied to the input element.
         */
        inputClass: {
            type: [String, Object, Array],
            default: 'form-control'
        },
        /**
         * The CSS class applied to the wrapper element.
         */
        wrapperClass: [String, Object, Array],
        /**
         * Whether Monday is the first day, or not.
         */
        mondayFirst: Boolean,
        /**
         * Display a button for clearing the dates.
         */
        clearButton: Boolean,
        /**
         * Use an icon for the clear button.
         */
        clearButtonIcon: String,
        /**
         * Dislay a calendar button.
         */
        calendarButton: Boolean,
        /**
         * The calendar button's icon.
         */
        calendarButtonIcon: String,
        /**
         * The calendar button's icon content.
         */
        calendarButtonIconContent: String,
        /**
         * If set, the datepicker is opened on that specific view.
         */
        initialView: String,
        /**
         * The disabled state.
         */
        disabled: Boolean,
        /**
         * The required state.
         */
        required: Boolean,
        /**
         * Whether to allow users to type the date, or not.
         */
        typeable: Boolean,
        /**
         * Use UTC for time calculations.
         */
        useUtc: Boolean,
        /**
         * If set, the lower-level views will not be shown.
         */
        minimumView: {
            type: String,
            default: 'day'
        },
        /**
         * If set, the higher-level views will not be shown.
         */
        maximumView: {
            type: String,
            default: 'year'
        }
    }
}
</script>

<style lang="scss">
    $white: #fff;
    $black: #000;

    $color-silver-sand: #c3c7cc;
    $color-primary: #007bff;
    $color-shuttle-gray: #5a6169;
    $color-porcelain: #eceeef;

    $border-color: transparent;
    $border-radius-default: .375rem;
    $transition-default: all 250ms cubic-bezier(.27,.01,.38,1.06);
    $font-system: -apple-system, BlinkMacSystemFont,  "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif, "Apple Color Emoji", "Segoe UI Emoji", "Segoe UI Symbol";

    $datepicker-color: $color-shuttle-gray;
    $datepicker-min-width: 10rem;
    $datepicker-background-color: $white;
    $datepicker-border-radius: $border-radius-default;
    $datepicker-font-size: 1rem;
    $datepicker-padding-x: 22px;
    $datepicker-padding-y: 20px;
    $datepicker-font-weight: 300;
    $datepicker-border: 1px solid rgba($black,.05);
    $datepicker-drop-shadows: 0 0.5rem 4rem rgba($black,.11),
                            0 10px 20px rgba($black,.05),
                            0 2px 3px rgba($black,.06);

    $datepicker-cell-width: 36px;
    $datepicker-cell-height: 36px;
    $datepicker-cell-hover-color: $color-porcelain;
    $datepicker-cell-line-height: 2;
    $datepicker-cell-font-size: 1rem;

    .vdp-datepicker__calendar {
        color: $datepicker-color;
        padding: $datepicker-padding-y $datepicker-padding-x;
        min-width: $datepicker-min-width;
        font-size: $datepicker-font-size;
        font-weight: $datepicker-font-weight;
        font-family: $font-system;
        background-color: $datepicker-background-color;
        border: $datepicker-border;
        border-radius: $datepicker-border-radius;
        box-shadow: $datepicker-drop-shadows;
        border: 1px solid rgba($black,.15) !important;

        // Header
        header {
            display: flex;
            padding-bottom: 10px;

            span {
                transition: $transition-default;
                border-radius: $border-radius-default;
                font-weight: 500;

                &.next:after {
                    border-left-color: $color-silver-sand;
                }

                &.prev:after {
                    border-right-color: $color-silver-sand;
                }
            }
        }

        // Header elements and specific calendar cells.
        header span,
        .cell.day:not(.disabled):not(.blank), .cell.month, .cell.year {
            &:hover {
                background-color: $datepicker-cell-hover-color;
                border-color: $border-color !important;
            }
        }

        // Cells
        .cell {
            line-height: $datepicker-cell-line-height;
            font-size: $datepicker-cell-font-size;
            border-radius: $border-radius-default;
            transition: $transition-default;
            border-color: $border-color;
            height: auto;

            // Day headers
            &.day-header {
                font-weight: 500;
            }

            // Days
            &.day {
                width: $datepicker-cell-width;
                height: $datepicker-cell-height;
                border-radius: 50%;
            }

            // Months
            &.month,
            &.year {
                height: $datepicker-cell-height;
                font-size: 12px;
                line-height: 33px;
            }

            // Selected
            &.selected,
            &.highlighted.selected {
                background: $color-primary !important;
                color: $white;
                &:hover {
                    background: darken($color-primary, 5) !important;
                    border-color: $border-color !important;
                }
            }

            &.highlighted {
                background: lighten($color-primary, 45) !important;
                &:hover {
                    border-color: $border-color !important;
                }
            }
        }
    }
</style>



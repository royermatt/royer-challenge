<template>
    <div id="datepicker" />
</template>

<script>
    import Litepicker from 'litepicker';
    import moment from 'moment';
    
    export default {
        props: {
            value: Object,
        },
        mounted() {
            //window.disableLitepickerStyles = true;
            const picker = new Litepicker({
                element: document.getElementById('datepicker'),
                singleMode: false,
                inlineMode: true,
                autoApply: false,
                startDate: (this.value) ? this.value.startDate : null,
                endDate: (this.value) ? this.value.endDate : null,
                firstDay: 0,
                format: "M/D/YY",
                tooltipText: {
                    one: 'day',
                    other: 'days'
                },
                tooltipNumber: (totalDays) => {
                    return totalDays - 1;
                }
            });
            picker.on('button:apply', (startDate, endDate) => {
                this.$emit('input', {
                    startDate: moment(startDate.dateInstance).startOf('day'),
                    endDate: moment(endDate.dateInstance).endOf('day'),
                });
            });
            picker.on('button:cancel', () => {
                picker.clearSelection();
                this.$emit('input', { startDate: null, endDate: null });
                this.$emit('cleared');
            });
        }
    }
</script>

<style lang="scss">
    #datepicker {
        margin: 30px;
    }
    .litepicker .container__footer .preview-date-range {
        margin-right: 25px;
    }
</style>
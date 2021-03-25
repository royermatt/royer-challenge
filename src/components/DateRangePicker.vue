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
  :root {
    --litepicker-container-months-color-bg: #001b34;
    --litepicker-container-months-box-shadow-color: #000;
    --litepicker-footer-color-bg: #001b34;
    --litepicker-footer-box-shadow-color: #000;
    --litepicker-month-header-color: #dedede;
    --litepicker-button-prev-month-color-hover: #0e75d2;
    --litepicker-button-next-month-color-hover: #0e75d2; 
    --litepicker-day-color: #dedede;
    --litepicker-day-color-hover: #0e75d2;
    --litepicker-is-in-range-color: hsla(0,0%,100%,.1);
    --litepicker-is-start-color-bg: #0e75d2;
    --litepicker-is-end-color-bg: #0e75d2;
    --litepicker-button-cancel-color-bg: #e51937;
    --litepicker-button-apply-color-bg: #00529b;
    --litepicker-button-reset-color-hover: #0e75d2;
    --litepicker-highlighted-day-color: #333;
  }
  #datepicker {

    .litepicker {

      .container__footer {

        .preview-date-range {
          text-align: left;
          position: absolute;
          left: 20px;
          bottom: 17px;
        }

        .button-cancel {
          background: none;
          padding: 7px 10px;
        }

        .button-apply {
          padding: 7px 10px;
        }
      }
    }
  }
</style>
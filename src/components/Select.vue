<template>
  <div class="select" @click="showList($event)" v-bind:class="{ active: isActive }" @focusout="handleFocusOut" tabindex="0" >
    <div class="select-label">
      <span class="select-title" v-if="value === null">{{ placeholder }}</span>
      <span class="select-title selected" v-if="value !== null">{{ selected.title }}</span>
      <span class="select-arrow"></span>
    </div>
    <ul class="select-list">
      <template v-for="(option, k) in options">
        <li v-bind:class="{ active: value === option.value }" :value="option.value" v-bind:key="k" @click="handleSelection($event)">{{ option.title }}</li>
      </template>
    </ul>
  </div>
</template>

<script>
  export default {
    name: "Select",
    props: {
      placeholder: String,
      options: Array,
      value: String,
    },
    data() {
      return {
        isActive: false,
        selected: {
          title: null,
          value: null,
        },
      };
    },
    methods: {
      showList(event) {
        const value = event.target.getAttribute("value");

        let index = this.options.findIndex((option) => option.value === value);
        if (index > -1) {
          this.isActive = !this.isActive;
        }
      },
      handleSelection(event) {
        const value = event.target.getAttribute("value");
        const title = event.target.textContent;

        let index = this.options.findIndex((option) => option.value === value);
        if (index > -1) {
          this.selected.title = title;
          this.selected.value = value;
          this.$emit("input", value);
        }
      },
      handleFocusOut() {
        this.isActive = false;
      },
    },
    mounted() {
      if (this.value) {
        let index = this.options.findIndex((option) => option.value === this.value);
        if (index > -1) {
          this.selected.title = this.options[index].title;
          this.selected.value = this.options[index].value;
        }
      }
    },
  };
</script>

<style lang="scss">
  .select {
    z-index: 1;
    outline: none;
    width: 100%;
    -webkit-user-select: none;
    -moz-user-select: none;
    -ms-user-select: none;
    user-select: none;
    cursor: pointer;
    position: relative;
    display: inline-block;
    padding: 6px 0;
    text-align: left;
    flex-grow: 1;
    margin-left: 16px;
    max-width: 140px;

    &:first-of-type {
      margin-left: 0;
    }

    .select-label {
      position: relative;
      margin: 0;
      font-weight: 500;
      white-space: nowrap;
      overflow: unset;
      text-overflow: unset;
      padding: 0;
      border-bottom: 1px solid #00529b;
      padding: 6px;
      max-width: 116px;
      min-width: 60px;
      flex-grow: 1;

      .select-title {
        width: calc(100% - 22px);
        text-overflow: ellipsis;
        white-space: nowrap;
        overflow: hidden;
        text-transform: capitalize;
        display: inline-block;
        opacity: 0.6;
        color: #fff;
        font-size: 16px;

        &.selected {
          opacity: 1;
        }
      }
    }

    .select-list {
      display: none;
      list-style: none;
      margin: 0;
      padding: 0;
      background: #002241;
      color: #ffffff;
      font-size: 14px;
      position: absolute;
      width: 100%;

      li {
        padding: 8px 6px;

        &.active,
        &:hover {
          background: #00529b;
        }
      }
    }

    .select-arrow {
      position: absolute;
      right: 16px;
      background: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIyNCIgaGVpZ2h0PSIyNCI+PHBhdGggZmlsbD0iIzBlNzVkMiIgZmlsbC1ydWxlPSJldmVub2RkIiBkPSJNNi45MTggOUwxMiAxMy44MyAxNy4wODIgOWwxLjY2OCAxLjU4NUwxMiAxN2wtNi43NS02LjQxNXoiLz48L3N2Zz4=) 50% no-repeat;
      width: 20px;
      height: 20px;
      transition: 0.3s;
      right: 0;

      &.select-arrow-up {
        transform: rotate(-180deg);
      }
      &.select-arrow-right {
        transform: rotate(-90deg);
      }
      &.select-arrow-left {
        transform: rotate(-270deg);
      }
    }

    &.active {
      background: #002241;

      .select-label .select-arrow {
        transform: rotate(-180deg);
      }
      .select-list {
        display: block;
      }
    }
  }
</style>

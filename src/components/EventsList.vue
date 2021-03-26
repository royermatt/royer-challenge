<template>
  <div id="events">
    <template v-if="loading === false">
        <template v-if="result.items && result.items.length > 0">
            <template v-if="theme === 'default'">
                <table>
                    <thead>
                        <tr><th>Key</th><th>Headline</th><th>SubHeadline</th><th>Start Time</th></tr>
                    </thead>
                    <tbody>
                        <tr v-for="event in result.items" :key="event.key">
                            <td class="key">{{ event.key }}</td>
                            <td class="headline"><a :href=event.site_url>{{ event.headline }}</a></td>
                            <td class="subheadline">{{ event.subheadline }}</td>
                            <td class="date">{{ moment(event.date).local().format("MMMM Do, YYYY h:mm A") }}</td>
                        </tr>
                    </tbody>
                </table>
            </template>
            <template v-if="theme === 'nfhs'">
                <template v-for="event in result.items">
                    <a class="event-card"  :href=event.site_url :key="event.key">
                        <span class="key">{{ event.key }}</span>
                        <span class="headline">{{ event.headline }}</span>
                        <span class="subheadline">{{ event.subheadline }}</span>
                        <span class="date">{{ moment(event.date).local().format("MMMM Do, YYYY h:mm A") }}</span>
                    </a>
                </template>
            </template>
            <p class="item-count">Showing {{result.items.length}} of {{result.total.toLocaleString()}} {{(result.items.length === 1) ? " result" : " results" }}</p>
        </template>
        <template v-if="!result.items || result.items.length === 0">
            <div calss="no-results">There are no results</div>
        </template>
    </template>
    <template v-if="loading === true">
        <div calss="loading">Loading...</div>
    </template>
  </div>
</template>

<script>
  import Vue from 'vue';
  import moment from 'moment';
  
  Vue.prototype.moment = moment;

  export default {
    name: 'EventsList',
    props: {
      assocation: String,
      dateRange: Object,
      theme: String,
    },
    data() {
        return {
            loading: true,
            result: {}
        }
    },
    methods: {
        async fetchUpcomingEvents() {
          this.loading = true;

          let url = "https://challenge.nfhsnetwork.com/v2/search/events/upcoming?card=true&size=50&start=0";

          if(this.assocation) {
            url += "&state_association_key=" + this.assocation;
          }

          if(this.dateRange.startDate) {
            url += "&from=" + moment(this.dateRange.startDate).toISOString();
          }

          if(this.dateRange.endDate) {
            url += "&to=" + moment(this.dateRange.endDate).toISOString();
          }

          fetch(url)
            .then(response => response.json())
            .then(data => {
                this.result = data;
                this.loading = false;
            });
        },
    },
    watch: {
        assocation: function() {
          this.fetchUpcomingEvents();
        },
        dateRange: function() {
          this.fetchUpcomingEvents();
        }
    },
    mounted() {
      this.fetchUpcomingEvents();
    },
  }
</script>

<style lang="scss">
  #events {
    margin: 30px 15px;

    table {
      color: rgba(255,255,255,0.8);
      text-align: left;
      border-collapse: collapse;
      margin: 0 auto;

      a {
        text-decoration: none;
        color: #ffffff;

        &:hover {
          text-decoration: underline;
        }
      }

      th, td {
        padding: 10px 15px;
      }
              
      & th:nth-child(1),
      & td:nth-child(1),
      & th:nth-child(3),
      & td:nth-child(3) {
        display: none;
      }

      @media screen and (min-width: 700px) {
        & th:nth-child(3),
        & td:nth-child(3) {
          display: block;
        }
      }

      @media screen and (min-width: 900px) {
        & th:nth-child(1),
        & td:nth-child(1) {
          display: block;
        }

        .date {
          min-width: 170px;
        }
      }

      tbody {
        tr {
          outline: 1px solid #efefef;
          background-color: #001b34;
          transition: all .2s ease-in-out;

          &:hover {
            transform: scale(1.05);
          }

          .headline {
            font-weight: bold;
          }
          .key,
          .subheadline,
          .date {
            font-size: 0.8em;
          }

          td {
            &:first-child {
              border-left-width: 1px;
            }
            &:last-child {
              border-right-width: 1px;
            }
          }
        }
      }
    }

    .item-count {
      letter-spacing: 1px;
      font-size: 0.8em;
      margin-top: 30px;
    }
    
    @media screen and (min-width: 500px) {
      margin: 30px;
    }
    
    .event-card {
      border: 1px solid hsla(0,0%,100%,.3);
      border-radius: 3px;
      background: #001b34;
      padding: 20px 20px 40px 20px;
      display:inline-block;
      width: 100%;
      max-width: 380px;
      min-height: 220px;
      margin: 0 20px 20px 0;
      text-decoration: none;
      color: #ffffff;
      position: relative;
      transition: all .2s ease-in-out;
      vertical-align: top;
      box-sizing: border-box;
      
      &:hover {
        transform: scale(1.05);
      }

      .key {
        display: block;
        font-size: 0.8em;
        position: absolute;
        bottom: 20px;
        left: 20px;
        color: #aaaaaa;
      }

      .headline {
        font-size: 1.3em;
        display: block;
        font-weight: bold;
        margin: 10px 0;
        text-align: left;
      }

      .subheadline {
        font-size: 0.9em;
        display: block;
        text-align: left;
        color: #aaaaaa;
      }

      .date {
        display: block;
        text-align: right;
        border-top: 1px solid hsla(0,0%,100%,.3);
        margin-top: 20px;
        padding: 10px 5px 0 5px;
        font-size: 0.8em;
        position: absolute;
        bottom: 20px;
        left: 20px;
        right: 20px;
      }
    }
  }
</style>
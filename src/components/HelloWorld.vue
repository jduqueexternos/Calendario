<template>
  <div class="calendarDiv">
    <v-flex>
      <v-layout align-center justify-center>
        <v-btn fab text color="grey darken-2" @click="prev">
          <v-icon x-large>{{ arrowLeft }}</v-icon>
        </v-btn>
        <v-toolbar-title v-if="$refs.calendar">{{
          $refs.calendar.title
        }}</v-toolbar-title>
        <div id="divCenter">
          <v-btn fab text color="grey darken-2" @click="next">
            <v-icon x-large>{{ arrowRight }}</v-icon>
          </v-btn>
        </div>
      </v-layout>
      <v-sheet height="100%">
        <v-calendar
          ref="calendar"
          v-model="focus"
          type="category"
          first-interval="6"
          interval-count="16"
          category-show-all
          :categories="categories"
          :events="listEvents"
          category-hide-dynamic
          :event-color="getEventColor"
          event-text-color="white"
          @click:event="showEvent"
          @mouseenter:time-category="intervalCategory"
          @click:time="createEvent"
          @mouseleave:event="leaveEvent"
          @mouseenter:event="EnterEvent"
        >
        <template v-slot:day-body="{ date, week }">
            <div
              class="v-current-time"
              :class="{ first: date === week[0].date }"
              :style="{ top: nowY }"
            ></div>
          </template>
        </v-calendar>
      </v-sheet>
    </v-flex>
    <v-dialog v-model="show" width="800px">
      <v-card elevation="2" style="padding: 20px; width: auto;">
        <v-autocomplete
          v-model="selectedEvent.category"
          :items="categories"
          label="Trabajador"
        ></v-autocomplete>
        <v-autocomplete
          v-model="selectedEvent.work"
          :items="works"
          label="Trabajo"
        ></v-autocomplete>
        <v-text-field
          v-model="selectedEvent.name"
          label="Cliente"
        ></v-text-field>
        <v-row justify="space-around">
          <v-col style="width: 350px; flex: 0 1 auto;">
            <h2>Inicio:</h2>
            <v-time-picker
              v-model="selectedEvent.start"
              max="20:00"
              min="05:30"
            ></v-time-picker>
          </v-col>

          <v-col style="width: 350px; flex: 0 1 auto;">
            <h2>Fin:</h2>
            <v-time-picker
              v-model="selectedEvent.end"
              max="20:00"
              min="05:30"
            ></v-time-picker>
          </v-col>
        </v-row>
        <v-card-actions>
          <v-btn
            color="blue"
            variant="text"
            @click="Guardar()"
          >
            Guardar
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </div>
</template>
<script>
import { mdiChevronRight, mdiChevronLeft } from "@mdi/js";

export default {
  components: {
  },
  data: () => ({
    focus: "",
    selectedEvent: {},
    selectedElement: null,
    selectedOpen: null,
    arrowRight: mdiChevronRight,
    arrowLeft: mdiChevronLeft,
    isClient: false,
    targetCategory: null,
    enter: null,
    close: null,
    startTime: null,
    listEvents: [{
            name: "name",
            start: new Date('2024-04-02 9:00:00'),
            end: new Date('2024-04-02 10:00:00'),
            color: "#1F32BB",
            category: 'Esperanza',
            details: "manos",
            timed: true
          },{
            category: "Yolanda",
            start: new Date("2024-04-02 13:15"),
            end: new Date("2024-04-02 14:15"),
            color: "#1F32BB",
            work: "Corte",
            name: "Valentina",
            details: "corte",
            timed: true
          }],
    categories: ['Yasmin', 'Esperanza','Emilcen', 'Yolanda', 'Diana'],
    works:['Manicure','Pedicure','Corte','Blower','Depilación','Masaje','Iluminaciones','Color'],
    ready: false,
    show: false,
  }),
  mounted() {
    // Method that creates the categories from the users
    // this.mountCategory();
    this.ready=true
    // this.scrollToTime()
    // this.updateTime()
  },
  computed: {
    // ...mapState(["events", "activeEvent", "category", "activeUser"]),
    cal () {
        return this.ready ? this.$refs.calendar : null
      },
      nowY () {
        return this.cal ? this.cal.timeToY(this.cal.times.now) + 'px' : '-10px'
      }
  },
  created() {
    // this.getEvents();
    // this.getCategories();
  },
  methods: {
    // ...mapMutations([
    //   "setActiveEvent",
    //   "mountCategory",
    //   "setEvent",
    //   "setSelectedEvent",
    //   "setEditEvent",
    // ]),
    async deleteEvent() {
      // const response = await EventsService.deleteEvent(
      //   this.selectedEvent.id_event
      // )
      //   .then((response) => {
      //     this.getEvents();
      //     this.selectedOpen = false;
      //   })
      //   .catch((e) => {
      //     console.log(e);
      //   });
    },
    async addEvent(data) {
      console.log(data)
      // const response = await EventsService.addEvent(data);
    },
    async getCategories() {
      // const response = await UsersService.getWorkers()
      //   .then((response) => {
      //     for (var i = 0; i < response.data.data.length; i++) {
      //       this.categories.push(response.data.data[i].user_name);
      //     }
      //   })
      //   .catch((e) => {
      //     console.log(e);
      //   });
    },
    async updateEvent() {
      if (this.selectedEvent.color != "#8E8E8E") {
        var service = {
          client_name: this.selectedEvent.client_name,
          worker_name: this.selectedEvent.category,
          service_date_start: this.selectedEvent.start,
          service_date_end: this.selectedEvent.end,
          service_color: "#8E8E8E",
          service_name: this.selectedEvent.name,
          service_description: this.selectedEvent.description,
          service_price: parseFloat(this.selectedEvent.price),
          service_status: "Completed",
        };
        // console.log(this.serviceUpdate);
        console.log(service)
        // const response = await EventsService.updateEvent(
        //   this.selectedEvent.id_event,
        //   service
        // )
        //   .then((response) => {
        //     console.log(response);
        //     this.selectedOpen = false;
        //     this.getEvents();
        //   })
        //   .catch((e) => {
        //     console.log(e);
        //   });
      }
    },
    async getEvents() {
      this.listEvents = [];
      // const response = await EventsService.getEvents()
      //   .then((response) => {
      //     if (response.data != "") {
      //       this.listEvents = response.data.data.map((item) => {
      //         return {
      //           id_event: item.id_service,
      //           client_name: item.client_name,
      //           category: item.worker_name,
      //           start: item.service_date_start,
      //           end: item.service_date_end,
      //           color: item.service_color,
      //           name: item.service_name,
      //           description: item.service_description,
      //           price: item.service_price,
      //           status: item.service_status,
      //         };
      //       });
      //     }
      //   })
      //   .catch((e) => {
      //     console.log(e.response.data);
      //   });
    },
    // Function that returns the color of the selected event
    getEventColor(event) {
      return event.color;
    },
    // Function that shows the ChangeEvent component
    editEvent() {
      if (this.selectedEvent.color != "#8E8E8E") {
        this.setEditEvent(true);
        this.$refs.childComponent.setVariables();
        this.setActiveEvent(true);
      }
    },
    // Function that changes the calendar day
    setToday() {
      this.focus = "";
    },
    // Function that saves the category that the mouseenter event returns: time-category
    intervalCategory(nativeEvent) {
      this.targetCategory = nativeEvent.category;
      // console.log(event)
    },
    // Function to create event
    createEvent(nativeEvent) {
      this.selectedEvent.category = this.targetCategory
      this.selectedEvent.start = (String(nativeEvent.hour)+':'+this.calculateMinute(Number(nativeEvent.minute)))
      this.selectedEvent.end = (String(Number(nativeEvent.hour) + 1)+':'+this.calculateMinute(Number(nativeEvent.minute)))
      this.selectedEvent.day = nativeEvent.date
      this.show = true
    },
    Guardar(){
      console.log('event', this.selectedEvent)
      var event = {
            category: this.selectedEvent.category,
            start: new Date(this.selectedEvent.day + " " + this.selectedEvent.start),
            end: new Date(this.selectedEvent.day + " " + this.selectedEvent.end),
            color: "#1F32BB",
            work: this.selectedEvent.work,
            name: this.selectedEvent.name,
            timed: true
          }
      this.listEvents.push(event)
      this.show = false
      this.event = {}
    },
    calculateMinute(minute) {
      if (minute >= 0 && minute < 14) {
        return '00'
      } else if(minute >= 15 && minute < 29) {
        return '15'
      } else if(minute >= 30 && minute < 44) {
        return '30'
      } else {
        return '45'
      }
    },
    intervalMinute(value, bottom, top) {
      var result = "";
      if (value < bottom + 7) {
        result = bottom;
      } else {
        result = top;
      }
      return result;
    },
    // Function to calculate 1 hour after the entered time
    calculateHour(hour) {
      var result = "";
      var val = 0;
      if (hour.substring(0, 1) == "0") {
        if (hour.substring(1, 2) == "9") {
          result = "10:" + hour.substring(3, 5);
        } else {
          val = 1 + parseInt(hour.substring(1, 2));
          result = "0" + val + ":" + hour.substring(3, 5);
        }
      } else if (hour.substring(0, 1) == "1") {
        if (hour.substring(1, 2) == "9") {
          result = "20:" + hour.substring(3, 5);
        } else {
          val = 1 + parseInt(hour.substring(1, 2));
          result = hour.substring(0, 1) + val + ":" + hour.substring(3, 5);
        }
      } else if (hour.substring(0, 1) == "2") {
        val = 1 + parseInt(hour.substring(1, 2));
        result = hour.substring(0, 1) + val + ":" + hour.substring(3, 5);
      }
      return result;
    },
    // Function that changes the enter bool if the mouse exits an event
    leaveEvent() {
      this.enter = false;
    },
    // Function that changes the bool of enter if the mouse enters an event
    EnterEvent() {
      this.enter = true;
    },
    // Function that moves the calendar to the left
    prev() {
      this.$refs.calendar.prev();
    },
    // Function that moves the calendar to the right
    next() {
      this.$refs.calendar.next();
    },
    // Function that saves the pointers returned by the showEvent event in variables
    showEvent({ nativeEvent, event }) {

      const open = () => {
        this.selectedEvent = event;
        this.setSelectedEvent(event);
        this.selectedElement = nativeEvent.target;
        setTimeout(() => (this.selectedOpen = true), 10);
      };
      if (this.selectedOpen) {
        this.selectedOpen = false;
        setTimeout(open, 10);
      } else {
        open();
      }
      nativeEvent.stopPropagation();
    },
    minDate(){
      if(this.selectedEvent.start == undefined || this.selectedEvent.start == ''){
        return '5:30'
      }
      else { return this.selectedEvent.start }
    },
    getCurrentTime () {
        return this.cal ? this.cal.times.now.hour * 60 + this.cal.times.now.minute : 0
      },
      scrollToTime () {
        const time = this.getCurrentTime()
        const first = Math.max(0, time - (time % 30) - 30)

        this.cal.scrollToTime(first)
      },
      updateTime () {
        setInterval(() => this.cal.updateTimes(), 60 * 1000)
      }
  },
};
</script>
<style scoped>
#divCenter {
  margin-right: -37px;
}
.calendarDiv {
  margin-top: 75px;
}
.v-current-time {
  height: 2px;
  background-color: #ea4335;
  position: absolute;
  left: -1px;
  right: 0;
  pointer-events: none;

}
</style>
        
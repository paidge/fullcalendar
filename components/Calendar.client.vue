<script setup>
// https://github.com/vkurko/calendar
import Calendar from "@event-calendar/core";
import "@event-calendar/core/index.css";
import Interaction from "@event-calendar/interaction";
import TimeGrid from "@event-calendar/resource-time-grid";

const props = defineProps({
  date: String,
});
const calendar = ref();

watch(
  () => props.date,
  (newValue) => {
    calendar.value.setOption("date", newValue);
  }
);

onMounted(async () => {
  await nextTick();
  calendar.value = new Calendar({
    target: document.getElementById("ec"),
    props: {
      plugins: [TimeGrid, Interaction],
      options: {
        slotMinTime: "09:30:00",
        slotMaxTime: "18:30:00",
        slotDuration: "00:30:00",
        // height: "500px",
        // slotHeight: 36,
        view: "resourceTimeGridDay",
        allDaySlot: false,
        headerToolbar: { start: "", center: "", end: "" },
        buttonText: (text) => ({
          ...text,
          today: "Aujourd'hui",
          next: "Jour suivant",
          prev: "Jour précédent",
        }),
        // Quand on clique sur un horaire du calendrier
        dateClick: (info) => {
          console.log(info);
        },
        // Quand on clique sur un event
        eventClick: (info) => {
          console.log(info);
        },
        // Après avoir déplacé un évènement
        eventDrop: (info) => {
          console.log(info);
        },
        // Après avoir modifié la durée d'un event
        eventResize: (info) => {
          console.log(info);
        },
        // Quand on clique pour créer un évènement
        noEventsClick: (info) => {
          console.log(info);
        },
        // Quand on change de date
        datesSet: (info) => {
          console.log(info);
        },
        datesAboveResources: true,
        dayHeaderFormat: (date) => {
          return date.toLocaleString("fr-FR", {
            weekday: "long",
            year: "numeric",
            month: "long",
            day: "numeric",
          });
        },
        resources: [
          { id: 1, title: "Studio A" },
          { id: 2, title: "Studio B" },
          { id: 3, title: "Studio C" },
        ],
        events: createEvents(),
      },
    },
  });
});

function createEvents() {
  let days = [];
  for (let i = 0; i < 7; ++i) {
    let day = new Date();
    let diff = i - day.getDay();
    day.setDate(day.getDate() + diff);
    days[i] =
      day.getFullYear() +
      "-" +
      _pad(day.getMonth() + 1) +
      "-" +
      _pad(day.getDate());
  }

  return [
    {
      start: days[0] + " 00:00",
      end: days[0] + " 09:00",
      resourceId: 1,
      display: "background",
    },
    {
      start: days[1] + " 12:00",
      end: days[1] + " 14:00",
      resourceId: 2,
      display: "background",
    },
    {
      start: days[2] + " 17:00",
      end: days[2] + " 24:00",
      resourceId: 1,
      display: "background",
    },
    {
      start: days[0] + " 10:00",
      end: days[0] + " 14:00",
      resourceId: 1,
      title: "The calendar can display background and regular events",
      color: "#FE6B64",
    },
    {
      start: days[2] + " 09:00",
      end: days[2] + " 13:00",
      resourceId: 2,
      title:
        "Events can be assigned to resources and the calendar has the resources view built-in",
      color: "#779ECB",
    },
    {
      start: days[3] + " 14:00",
      end: days[3] + " 20:00",
      resourceId: 1,
      title: "",
      color: "#FE6B64",
    },
    {
      start: days[3] + " 15:00",
      end: days[3] + " 18:00",
      resourceId: 1,
      title: "Overlapping events are positioned properly",
      color: "#779ECB",
    },
    {
      start: days[5] + " 10:00",
      end: days[5] + " 16:00",
      resourceId: 2,
      title: {
        html: "You have complete control over the <i><b>display</b></i> of events…",
      },
      color: "#779ECB",
    },
    {
      start: days[5] + " 14:00",
      end: days[5] + " 19:00",
      resourceId: 2,
      title: "…and you can drag and drop the events!",
      color: "#FE6B64",
    },
    {
      start: days[5] + " 18:00",
      end: days[5] + " 21:00",
      resourceId: 2,
      title: "",
      color: "#B29DD9",
    },
  ];
}

function _pad(num) {
  let norm = Math.floor(Math.abs(num));
  return (norm < 10 ? "0" : "") + norm;
}
</script>

<template>
  <div style="max-width: 50%">
    <div id="ec"></div>
  </div>
</template>

<style>
.ec-toolbar,
.ec-header {
  position: sticky;
  z-index: 10;
  background: white;
}

.ec-toolbar {
  top: 0;
  margin-bottom: 0;
}
.ec-header {
  top: 0;
}

.ec-day {
  text-transform: capitalize;
}

/* .ec-time,
.ec-line {
  height: 36px;
} */
</style>

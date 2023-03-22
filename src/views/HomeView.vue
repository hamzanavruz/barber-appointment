<script setup>
</script>

<template>
  <div class="bg-slate-800 h-full p-8 pt-2 md:p-0  md:h-screen" :class="{ 'h-screen': showContainer == 'detail' }">
    <div class="flex justify-center">
      <img src="../assets/logo.png" class="mt-5 w-2/3 md:w-1/4">
    </div>Name Surname
    <div class="justify-center items-center content-center bg-slate-800 mt-8" v-show="showContainer == 'list'">
      <ul class="grid grid-cols-3 md:grid-cols-6 gap-4 mb-20 md:mb-0 ">
        <li class="rounded-lg shadow-lg p-5 md:p-8 cursor text-center"
          :class="{ 'bg-red-400 text-white': item.status == 'active' }, { 'bg-white': item.status == 'empty' }"
          @click="selectHour(item)" v-for="item in hours">{{
            item.hour
          }}</li>
      </ul>
    </div>


    <div
      class="p-2 md:p-12 mt-28 md:p-0 flex  justify-center items-center content-center bg-slate-800 animate__animated animate__fadeIn"
      v-show="showContainer == 'detail'">

      <div class="w-full max-w-xs" v-show="selectedHour.status == 'empty'">
        <div class="bg-white shadow-md rounded-lg px-8 pt-6 pb-8 mb-4">
          <h6 class="font-bold mb-2">{{ selectedHour.hour }}</h6>
          <div class="mb-4">
            <label class="block text-gray-700 text-sm font-bold mb-2" for="username">
              Name Surname
            </label>
            <input
              class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline"
              id="username" type="text" placeholder="Name Surname" ref="newName">
          </div>

          <div class="">

            <button
              class="bg-blue-500 hover:bg-blue-700 text-white  py-2 px-3 rounded focus:outline-none focus:shadow-outline w-full"
              type="button" @click="addHour">
              Save
            </button>
            <button
              class="bg-red-500 hover:bg-red-700 text-white  py-2 px-3 rounded focus:outline-none focus:shadow-outline w-full mt-3"
              type="button" @click="(() => { showContainer = 'list' })">
              Close
            </button>
          </div>
        </div>

      </div>

      <div class="w-full max-w-xs" v-show="selectedHour.status == 'active'">
        <div class="bg-white shadow-md rounded-lg px-8 pt-6 pb-8 mb-4">
          <h6 class="font-bold mb-2">{{ selectedHour.hour }}</h6>
          <div class="mb-4">
            <label class="block text-gray-700 text-sm font-bold mb-2" for="username">
              Name Surname
            </label>
            <input
              class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline"
              id="username" type="text" placeholder="Name Surname" :value="selectedHour.name" ref="name">
          </div>

          <div class="">
            <button
              class="bg-blue-500 hover:bg-blue-500 text-white  py-2 px-3 rounded focus:outline-none focus:shadow-outline w-full"
              type="button" @click="updateHour">
              Save
            </button>
            <button
              class="bg-yellow-500 hover:bg-yellow-500 text-white  py-2 px-3 rounded focus:outline-none focus:shadow-outline w-full mt-3"
              type="button" @click="cancelHour">
              Cancel Appointment
            </button>
            <button
              class="bg-red-500 hover:bg-red-500 text-white  py-2 px-3 rounded focus:outline-none focus:shadow-outline w-full mt-3"
              type="button" @click="(() => { showContainer = 'list' })">
              Close
            </button>
          </div>
        </div>

      </div>

    </div>


  </div>
<div class="menu fixed bottom-0 w-full bg-slate-900 ">
  <ul class="flex gap-4 justify-center items-center content-center h-12 relative">
      <!--<li class="text-white rounded-lg bg-slate-800 p-2 w-16 text-center">
                    <a @click="(() => { showContainer = 'list' })"><i class="fa fa-home"></i></a>
                  </li>-->
      <li class="text-white rounded-lg bg-slate-800 p-2 w-16 text-center" @click="resetDay">
        <a><i class="fa fa-undo"></i></a>
      </li>
    </ul>
  </div>
</template>
<script>
import hours from '../data/hours.json'
export default {
  data() {
    return {
      hours: [],
      showContainer: 'list',
      selectedHour: {}
    }
  },
  methods: {
    selectHour(item) {
      this.showContainer = 'detail'
      this.selectedHour = item
    },
    addHour() {
      let name = this.$refs.newName.value;
      let hour = this.selectedHour.hour;
      let dateList = JSON.parse(localStorage.getItem("date"));
      let date = {
        "hour": hour,
        "status": "active",
        "name": name
      };
      let array = [];
      dateList.map((item) => {
        if (item.hour === hour) {
          array.push(date);
        } else {
          array.push(item);
        }
      });
      localStorage.setItem("date", JSON.stringify(array));
      this.showContainer = "list";
      this.$refs.newName.value = "";
      this.selectedHour = {};
      this.hours = JSON.parse(localStorage.getItem("date"));
    },
    updateHour() {
      let name = this.$refs.name.value;
      let hour = this.selectedHour.hour;
      let dateList = JSON.parse(localStorage.getItem("date"));
      let date = {
        "hour": hour,
        "status": "active",
        "name": name
      };
      let array = [];
      dateList.map((item) => {
        if (item.hour === hour) {
          array.push(date);
        } else {
          array.push(item);
        }
      });
      localStorage.setItem("date", JSON.stringify(array));
      this.showContainer = "list";
      this.$refs.name.value = "";
      this.selectedHour = {};
      this.hours = JSON.parse(localStorage.getItem("date"));
    },
    cancelHour() {

      Swal.fire({
        title: 'Are you sure you want to delete?',
        icon: 'question',
        showCancelButton: true,
        cancelButtonText: 'Cancel',
        confirmButtonText: 'Confirm',

      }).then((result) => {

        if (result.isConfirmed) {
          let dateList = JSON.parse(localStorage.getItem("date"));
          let hour = this.selectedHour.hour;
          let date = {
            "hour": hour,
            "status": "empty",
            "name": ""
          };
          let array = [];
          dateList.map((item) => {
            if (item.hour === hour) {
              array.push(date);
            } else {
              array.push(item);
            }
          });
          localStorage.setItem("date", JSON.stringify(array));
          this.showContainer = "list";
          this.$refs.name.value = "";
          this.selectedHour = {};
          this.hours = JSON.parse(localStorage.getItem("date"));
        }
      })
    },
    resetDay() {
      Swal.fire({
        title: 'Are you sure you want to delete all appointments?',
        icon: 'question',
        showCancelButton: true,
        cancelButtonText: 'Cancel',
        confirmButtonText: 'Confirm',

      }).then((result) => {
        localStorage.removeItem("date");
        location.reload();
      })
    }

  },
  mounted() {
    if (localStorage.getItem("date")) {
      this.hours = JSON.parse(localStorage.getItem("date"));
    } else {
      localStorage.setItem("date", JSON.stringify(hours));
      this.hours = hours;
    }
  }
}
</script>
<style>
.cursor {
  cursor: pointer;
}
</style>
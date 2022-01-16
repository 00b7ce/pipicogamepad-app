<template>
  <div class="deviceList">{{ msg }}</div>
  <!-- <ul> -->
  <!-- <li v-for="device in devices" v-bind:key="device.state">
            {{ device }}
        </li> -->
  <!-- </ul> -->
  <!-- <ul> -->
  <div v-for="(value, key) in report" :key="key">{{ value }},</div>
  <!-- </ul> -->
  <button v-on:click="openDevice">Open Device</button><br />
  <button v-on:click="closeDeive">Close Device</button>
</template>

<script lang="ts">
import { Options, Vue } from "vue-class-component";
import { defineComponent } from "vue";

export default defineComponent({
  name: "HIDList",

  data() {
    return {
      msg: "" as string,
      devices: [] as Array<any>,
      report: [] as Array<any>,
    };
  },
  created() {
    this.getHIDDeviceList();
  },

  beforeUnmount() {
    this.devices.forEach(async (device) => {
      if (device.opened) {
        // device.removeEventListener('inputreport', (e: any)=>{console.log(e)});
        device.removeEventListener("inputreport", (e: any) =>
          this.receiveReport(e)
        );
        await device.close();
      }
    });
  },

  methods: {
    async getHIDDeviceList() {
      const devices: any[] = await navigator.hid.getDevices();
      devices.forEach((device) => {
        this.devices.push(device);
      });
    },
    async openDevice() {
      const device = this.devices[0];
      // console.log(device);
      // if(device) {
      await device.open();
      // device.addEventListener('inputreport', (e: any)=>{console.log(e)});
      device.addEventListener("inputreport", (e: any) => this.receiveReport(e));
      this.msg = "Device opened.";
      // }
    },
    async closeDeive() {
      const device = this.devices[0];
      // device.removeEventListener('inputreport', (e: any)=>{console.log(e)});
      device.removeEventListener("inputreport", (e: any) =>
        this.receiveReport(e)
      );
      await device.close();
      this.msg = "Device closed.";
      // console.log(device);
    },

    receiveReport(e: any) {
      // console.log(e.data.buffer);
      this.report.length = 0;
      for (let i = 0; i < e.data.byteLength; i++) {
        this.report.push(e.data.getUint8(i));
      }
      // console.log(this.report);
    },
  },
});
</script>

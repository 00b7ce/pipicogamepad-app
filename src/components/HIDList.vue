<template>

  <div class="deviceList">{{ msg }}</div>

  <!-- <div v-for="device in devices" v-bind:key="device.state">{{ device.productName }}</div> -->

  <div>input:<a v-for="(value, key) in report" :key="key">{{ value }}, </a></div>
  <div>setting:<a v-for="(value, key) in report2" :key="key">{{ value }}, </a></div>

  <!-- <button v-on:click="getHIDDeviceList">Find Device</button><br /> -->
  <!-- <br/> -->
  <!-- <button v-on:click="openDevice">Open Device</button><br /> -->
  <!-- <br/> -->
  <!-- <button v-on:click="closeDeive">Close Device</button> -->
</template>

<script lang="ts">
import { Options, Vue } from "vue-class-component";
import { defineComponent } from "vue";

export default defineComponent({
  name: "HIDList",

  data() {
    return {
      productName : "MKB Gamepad" as string,
      msg: "" as string,
      devices: [] as Array<any>,
      report:  [] as Array<any>,
      report2: [] as Array<any>
    };
  },
  mounted() {
    this.getHIDDeviceList();
    navigator.hid.addEventListener('connect', (device :any) => this.getHIDDeviceList());
    navigator.hid.addEventListener('disconnect', (event :any) => this.disconnectClose(event));
  },

  beforeUpdate() {
    this.openDevice();
  },

  

  beforeUnmount() {
    this.devices.forEach(async (device) => {
      if (device.opened) {
        device.removeEventListener("inputreport", (e: any) => this.receiveReport(e));
        await device.close();
      }
    });
  },

  methods: {
    async getHIDDeviceList() {
      this.devices.length = 0;
      const devices: any[] = await navigator.hid.getDevices();
      devices.forEach((device) => {
        if(device.productName == this.productName) {
          this.devices.push(device);
        }
      });
      if(this.devices.length == 2) {
        this.msg = 'device found';
      }
    },

    disconnectClose(event: any) {
      console.log('device is disconnect');
      if(event.device.productName == this.productName) {
        event.device.removeEventListener("inputreport", (e: any) => this.receiveReport(e));
        this.devices.length = 0;
      }
    },

    async openDevice() {
      if(this.devices.length == 0) return;

      this.devices.forEach(async (device) => {
        if(!device.opened) {
          await device.open();
          device.addEventListener("inputreport", (e: any) => this.receiveReport(e));
        }
      });

      this.msg = "Device opened.";

    },

    async closeDeive() {
      this.devices.forEach(async (device) => {
        device.removeEventListener("inputreport", (e: any) => this.receiveReport(e));
        await device.close();
      });

      this.msg = "Device closed.";

    },

    receiveReport(e: any) {
      if(e.reportId == 4) {
        this.report.length = 0;
        for (let i = 0; i < e.data.byteLength; i++) {
          this.report.push(e.data.getUint8(i));
        }
      }
      else {
        this.report2.length = 0;
        for (let i = 0; i < e.data.byteLength; i++) {
          this.report2.push(e.data.getUint8(i));
        }
      }
    },

  },
});
</script>

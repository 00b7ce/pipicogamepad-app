<template>

  <div class="deviceList">{{ msg }}</div>

  <div v-for="device in devices" v-bind:key="device.state">{{ device.productName }}</div>
  <br/>
  <div>input:<a v-for="(value, key) in report" :key="key">{{ value }}, </a></div>
  <div>setting:<a v-for="(value, key) in report2" :key="key">{{ value }}, </a></div>
  <!-- <div v-for="(value, key) in report" :key="key">{{ value }},</div>
  <div v-for="(value, key) in report2" :key="key">{{ value }},</div> -->
  <br/>
  <button v-on:click="getHIDDeviceList">Find Device</button><br />
  <br/>
  <button v-on:click="openDevice">Open Device</button><br />
  <br/>
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
      report:  [] as Array<any>,
      report2: [] as Array<any>
    };
  },
  mounted() {
    this.getHIDDeviceList();
  },

  beforeUnmount() {
    this.devices.forEach(async (device) => {
      navigator.hid.addEventListener("connect", ({device}) => this.getHIDDeviceList());
      if (device.opened) {
        device.removeEventListener("inputreport", (e: any) => this.receiveReport(e));
        await device.close();
      }
    });
  },

  methods: {
    async getHIDDeviceList() {
      this.devices.length = 0;
      const devices: any[] = await navigator.hid.requestDevice({
        filters: [{
          vendorId: 0xcafe,
          productId: 0x4008,
        }]
      });
      devices.forEach((device) => {
        this.devices.push(device);
      });
    },

    async openDevice() {
      this.devices.forEach(async (device) => {
        await device.open();
        device.addEventListener("inputreport", (e: any) => this.receiveReport(e));
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

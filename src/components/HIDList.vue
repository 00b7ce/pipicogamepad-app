<template>
    <div class="deviceList">{{ msg }}</div>
    <ul>
        <li v-for="device in devices" v-bind:key="device.productName">
            {{ device }}
        </li>
    </ul>
</template>

<script lang="ts">
import { Options, Vue } from "vue-class-component";
import { defineComponent } from "vue";

export default defineComponent({
    name: 'HIDList',
    data() {
        return {
            msg: "test",
            devices: [] as Array<{productName:''}>
        }},
    created: function() {
        this.getHIDDeviceList();
    },
    methods: {
        methodTest() {
            return "Hello TypeScript"
        },
        async getHIDDeviceList(){
            const devices: any[] = await navigator.hid.getDevices();
            devices.forEach(device => {
                this.devices.push(device.productName);
            });
        }
    },
});

// async function getHIDDeviceList() {
//     const devices: any[] = await navigator.hid.getDevices();
//     devices.forEach(device => {
//         // grantedDeviceList += "<hr>" + device.productName + "</hr>";
//         console.log(device);
//     });

//     // const devices: HIDDevice[] = await navigator.hid.requestDevice({
//     //     filters: [{
//     //         // deviceのvendorとproductのID
//     //         //vendorId?: number;
//     //         //productId?: number;
//     //         vendorId: 0xcafe,
//     //         productId: 0x4008,
//     //     }]
//     // });
//     // const device = devices[0];
//     // await device.open();

//     // device.addEventListener("inputreport", (e)=>{console.log(e)});
// }

// export default class HIDList extends Vue {}
</script>
<template>
    <div class="deviceList">{{ msg }}</div>
</template>

<script lang="ts">
import { Options, Vue } from "vue-class-component";
import { defineComponent } from "vue";

export default defineComponent({
    name: 'Test',
    data() {
        return {
            msg: "test"
        }},
    created: function() {
        console.log(this.methodTest());
    },
    methods: {
        methodTest() {
            return "Hello TypeScript"
        }
    },
});

// interface ComputexMessage {
//     title: string,
//     okMessage: string,
//     cancelMessage: string
// }

// export default class HIDList extends Vue ({
//     private testText!: string,
//     data() {
//         return {
//             this.testText: "Hello TypeScript",
//         }},
// });

// export default {
//     name: 'ListHID',
//     data() {
//         return {
//             todos: [
//                 { text: 'Learn JavaScript' },
//                 { text: 'Learn Vue' },
//                 { text: 'Build something awesome' }
//             ]
//         }
//     }
// }
// let grantedDeviceList: string;
// getHIDDeviceList();

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
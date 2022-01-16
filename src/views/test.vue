<template>
  <div class="test">
    <!-- <img alt="Vue logo" src="../assets/logo.png" /> -->
    <HIDList />
  </div>
</template>

<script lang="ts">
import { Options, Vue } from "vue-class-component";
import HIDList from "@/components/HIDList.vue";

@Options({
  components: {
    HIDList,
  },
})
export default class Test extends Vue {}
</script>

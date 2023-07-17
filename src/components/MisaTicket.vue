<script setup>
import * as LiveChat from "@livechat/agent-app-sdk";
</script>
<template>
  <div>hello men</div>
</template>
<script>
export default {
  data() {
    return {
      test: 123,
    };
  },
  created() {
    console.log(12312321);
    this.embedPopup()
  },
  methods: {
    embedPopup() {
      LiveChat.createDetailsWidget().then((widget) => {
        // build your logic around the widget
        widget.on("customer_profile", (profile) => {
          // read the new profile
          console.log(profile);
        });
      });
    },
  },
};
</script>
<style scoped>
</style>
<script setup>
import * as LiveChat from "@livechat/agent-app-sdk";
import { Livechat } from "../object/Livechat";
import axios from "axios";
import { ref } from "vue";

const iframeURL = ref("https://testticket.misa.vn/livechat");
</script>
<template>
  <div>
    <iframe
      :src="iframeURL"
      frameborder="0"
      style="width: 100%; height: 100vh"
    ></iframe>
  </div>
</template>
<script>
export default {
  data() {
    return {
      frameURL: "https://testticket.misa.vn/livechat",
    };
  },
  async created() {
    await this.embedPopup();
    // await this.getEmbedPopup();
  },
  methods: {
    async embedPopup() {
      var cusData = {
        customerName: "", // Tên công ty
        phoneNumber: "", // Số điện thoại
        email: "", // Email
        fullName: "", // Tên đầy đủ
        taxCode: "", // Mã số thuế
        source: "", // Nguồn
        version: "", // Phiên bản
        BudgetCode: "", // Mã quan hệ ngân sách
      };
      var frameData = 'https://testticket.misa.vn/livechat';
      await LiveChat.createDetailsWidget().then((widget) => {
        // build your logic around the widget
        widget.on("customer_profile", (profile) => {
          cusData.customerName =
            profile.customVariables["Công ty" || "Cty" || undefined] || "";
          cusData.phoneNumber =
            profile.customVariables["Điện thoại"] ||
            profile.customVariables["Số điện thoại"] ||
            profile.customVariables["SĐT"] ||
            "";
          cusData.email = profile.email;
          cusData.fullName = profile.customVariables["Họ tên"] || "";
          cusData.taxCode =
            profile.customVariables["Mã số thuế"] ||
            profile.customVariables["MST"] ||
            "";
          cusData.source = profile.customVariables["Nguồn"] || "";
          cusData.version =
            profile.customVariables["Phiên bản"] || "";
          cusData.BudgetCode =
            profile.customVariables["Mã QHNS"] || "";
          console.log(456);
          // console.log(cusData);

          axios
            .post(
              `${import.meta.env.VITE_BASE_API_URL}${
                import.meta.env.VITE_TICKET_URL
              }`,
              cusData,
              {
                headers: {
                  Accept: "/",
                  "Content-Type": "application/json",
                },
              }
            )
            .then( (res) => {
              console.log(`fff ${JSON.stringify(cusData)}`);
              frameData = res.data;
            })
            .catch( (err) => {
              console.log(err);
            });
        });
      });

      this.iframeURL = frameData;
      console.log(this.iframeURL);
    },
    async getEmbedPopup() {
      console.log(`gêtmbedPopup ${JSON.stringify(cusData)}`);
      await axios
        .post(
          `${import.meta.env.VITE_BASE_API_URL}${
            import.meta.env.VITE_TICKET_URL
          }`,
          cusData,
          {
            headers: {
              Accept: "/",
              "Content-Type": "application/json",
            },
          }
        )
        .then(function (res) {
          console.log(`fff ${JSON.stringify(cusData)}`);
          this.iframeURL = res.data;
        })
        .catch(function (err) {
          console.log(err);
        });
    },
  },
};
</script>
<style scoped>
</style>
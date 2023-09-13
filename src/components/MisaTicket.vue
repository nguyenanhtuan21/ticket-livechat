<script setup>
import * as LiveChat from "@livechat/agent-app-sdk";
import { Livechat } from "../object/Livechat";
import axios from "axios";
import { ref } from "vue";

const iframeURL = ref("https://ticket.misa.vn/livechat");
</script>
<template>
  <div>
    <iframe
      :src="frameURL"
      frameborder="0"
      style="width: 100%; height: 100vh"
    ></iframe>
  </div>
</template>
<script>
export default {
  data() {
    return {
      frameURL: "https://ticket.misa.vn/livechat",
    };
  },
  async created() {
    await this.embedPopup();

    /////
    // console.log(`https://ticket.misa.vn/apis/open-api/livechat/request-link`);
    // var cusData = {
    //   customerName: "", // Tên công ty
    //   phoneNumber: "", // Số điện thoại
    //   email: "", // Email
    //   fullName: "", // Tên đầy đủ
    //   taxCode: "", // Mã số thuế
    //   source: "", // Nguồn
    //   version: "", // Phiên bản
    //   BudgetCode: "", // Mã quan hệ ngân sách
    // };
    // console.log(`getembedPopup ${JSON.stringify(cusData)}`);
    // await axios
    //   .post(
    //     `${import.meta.env.VITE_BASE_API_URL}${
    //       import.meta.env.VITE_TICKET_URL
    //     }`,
    //     cusData,
    //     {
    //       headers: {
    //         Accept: "/",
    //         "Content-Type": "application/json",
    //       },
    //     }
    //   )
    //   .then((res) => {
    //     console.log(`fffsss ${JSON.stringify(cusData)}`);
    //     console.log(typeof(res.data));
    //     // this.frameURL = {...res.data};
    //     console.log(this.frameURL+'?ver=23');
    //   })
    //   .catch((err) => {
    //     console.log(err);
    //   });
    /////
  },
  methods: {
    async embedPopup() {
      try {
        await LiveChat.createDetailsWidget().then((widget) => {
          // build your logic around the widget
          widget.on("customer_profile", (profile) => {
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

            if (profile && profile.customVariables) {
		try {
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
		profile.customVariables["TaxCode"] ||
                "";
              cusData.source = profile.customVariables["Nguồn"] || "";
              cusData.version = profile.customVariables["Phiên bản"] || "";
              cusData.BudgetCode = profile.customVariables["Mã QHNS"] || "";
              console.log(456);
		} catch(error) {}
            }
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
              .then((res) => {
                console.log(`fff ${JSON.stringify(cusData)}`);
                this.frameURL = res.data;
              })
              .catch((err) => {
                console.log(err);
              });
          });
        });
      } catch (error) {
        console.log(error);
      }
    },
    async getEmbedPopup() {},
  },
};
</script>
<style scoped>
</style>
<template>
  <h1>Enter 6 digit OTP</h1>
  <input
    v-for="(value, index) in otp"
    :key="index"
    v-model="otp[index]"
    maxlength="1"
    @input="handleInputChange(index, $event)"
  />
</template>
<script setup>
import { ref, defineEmits, watch } from "vue";

const emit = defineEmits(["printOtpString"]);

const otpSize = ref(6); //no of otp digit
const otp = ref(Array(otpSize.value).fill("")); // ['', '', '', '', '', '']

function handleInputChange(index, event) {
  let value = event.target.value.replace(/\D/g, ""); //Replace all non digit value by "";

  otp.value[index] = value;

  otp.value = [...otp.value]; //to trigger reactivity
}

watch(otp, () => {
  let isCompleted = otp.value.every((item) => item !== "" || null); //checking if all fields have value

  if (isCompleted) {
    let otpOutput = otp.value.join("");
    console.log(otpOutput);
    emit("printOtpString", otpOutput);
  }
});
</script>
<style scoped>
input {
  background-color: white;
  width: 50px;
  height: 50px;
  padding: 10px;
  margin: 10px;
  border-radius: 5px;
  color: black;
  font-size: 2em;
  text-align: center; /* horizontal */
  line-height: 30px; /* vertical alignment workaround */
}
</style>

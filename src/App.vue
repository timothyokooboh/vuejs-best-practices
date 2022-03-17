<script setup lang="ts">
import CustomerList from "./components/CustomerList.vue";
import { onErrorCaptured } from "vue";

const triggerError = () => {
  return Promise.reject({ message: "something went wrong in app.vue" });
};

</script>

<template>
  <CustomerList />
   <!-- <button @click="triggerError">Trigger error</button> -->

</template>

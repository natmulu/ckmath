<template>
    <MySweetSweetEditor ed="first" @update:content="inputReceived" />
    <MySweetSweetEditor ed="ss" @update:content="inputReceived" />
    <MySweetSweetEditor ed="as" @update:content="inputReceived" />
</template>

<script setup>
import MySweetSweetEditor from "./components/MySweetSweetEditor.vue";

function inputReceived(input) {
    console.log(input);
}
</script>

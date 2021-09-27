<template lang="pug">
  h2.msg
    slot
</template>

<script>
export default {
  name: "Message",
};
</script>

<style lang="scss" scoped>
.msg {
  color: #000000;
  font-size: 22px;
}
</style>

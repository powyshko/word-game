<template lang="pug">
  button.btn(:class='btnClass', v-on='$listeners', v-bind='$attrs')
    slot
</template>

<script>
export default {
  name: "BaseButton",
  props: {
    btnClass: {
      type: String,
      default: "btnPrimary",
    },
  },
};
</script>

<style lang="scss">
.btn {
  font-size: 14px;
  line-height: 20px;
  padding: 16px 22px 14px 22px;
  border-radius: 10px;
  display: inline-block;
  cursor: pointer;
}

.btnPrimary {
  color: #ffffff;
  background: #005aff;
  border: 1px solid #005aff;
}

.btnSucess {
  color: #ffffff;
  background: #34ff01;
  border: 1px solid #34ff01;
}
</style>

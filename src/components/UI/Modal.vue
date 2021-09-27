<template lang="pug">
  .modal-wrapper
    .modal-container(@click.stop='')
      .modal-body
        slot(name='body')
</template>

<script>
export default {
  name: "Modal",
};
</script>

<style scoped>
.modal-wrapper {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.35);
  z-index: 998;
}

.modal-container {
  background: #ffffff;
  border-radius: 10px;
  padding: 20px;
  position: relative;
  z-index: 999;
  font-size: 18px;
  width: 400px;
}

.modal-header {
  display: flex;
  align-items: flex-start;
  justify-content: space-between;
}

.modal-close {
  cursor: pointer;
}
</style>

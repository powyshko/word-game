<template lang="pug">
  table.table(v-if="resultGame.length > 0")
    thead
      tr
        th(@click="$emit('sort', 'level')") Номер раунда
        th(@click="$emit('sort', 'word')") Слово
        th(@click="$emit('sort', 'time')") Время выполнения
    tbody
      tr(v-for="item in resultGame" :key="item.level")
        td {{item.level}}
        td {{item.word}}
        td {{item.time}}
</template>

<script>
export default {
  name: "Table",
  props: {
    resultGame: {
      type: Array,
      default: () => [],
    },
  },
};
</script>

<style lang="scss" scoped>
.table {
  text-align: left;
  border-collapse: separate;
  border-spacing: 5px;
  background: #ece9e0;
  border: 16px solid #ece9e0;
  border-radius: 20px;
  width: 50%;
  margin: 50px auto;

  th {
    padding: 12px 10px;
    cursor: pointer;
  }

  td {
    padding: 10px;
    background: #ffffff;
  }
}
</style>

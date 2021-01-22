<template>
  <div id="storage">
    <h3>Склад</h3>
    <div v-if="groupedPotsByFruits.length">
      <div v-for="(group, index) in groupedPotsByFruits" :key="index" class="ui segment">
        <span class="ui ribbon label">{{ group.fruit }}  ({{ group.total }} г)</span>
        <div v-for="(pot, index) in group.pots" :key="index">
          Банка {{ pot.label }} ({{ pot.volume }}/{{ pot.filling }} г)
        </div>
      </div>
    </div>
    <span v-else style="color: grey;">Пусто.</span>
  </div>
</template>

<script>
export default {
  name: 'Storage',
  props: ['pots', 'fruits'],
  computed: {
    groupedPotsByFruits() {
      const groups = [];
      // группируем банки по типу фруктов
      for (let i = 0; i < this.fruits.length; i++) {
        let item = {};
        item.fruit = this.fruits[i];
        // берем только текущий тип
        item.pots = this.pots.filter((cur) => {
          return cur.fruit === item.fruit;
        });
        // суммируем вес
        item.total = item.pots.reduce((prev, cur) => {
          return prev + cur.filling
        }, 0);
        if (item.total) groups.push(item);
      }
      return groups;
    }
  }
}
</script>

<style>
#storage {
  background: #fff;
}
</style>

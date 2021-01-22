<script>
import Pot from './components/Pot.vue'
import Jam from './components/Jam.vue'
import Storage from './components/Storage.vue'

export default {
  name: 'App',
  components: {
    Pot, Jam, Storage
  },
  data() {
    return {
      fruits: ['Клубника', 'Вишня', 'Абрикос'],
      pots: [],
      jams: []
    }
  },
  computed: {
    // свободный объем банок (всего)
    freeVolume() {
      return this.pots.reduce((prev, cur) => {return cur.filling ? 0 : prev + cur.volume}, 0);
    }
  },
  methods: {
    newPotAdded(newPot) {
      this.pots.push(newPot);
    },
    newJamAdded(newJam) {
      if (newJam.volume < this.freeVolume) {
        this.jams.push(newJam);
        this.fillPots(newJam);
      } else {
        alert('НЕ ДОБАВЛЕНО! Недостаточно свободных банок.');
      }
    },

    // разливаем новое варенье по свободным банкам
    fillPots(jam) {
      let total = jam.volume; // текущий остаток
      // перебираем массив банок
      for (let i = 0; i < this.pots.length; i++) {
        // если банка уже заполнена, или остаток нулевой, тогда пропускаем
        if (!this.pots[i].filling && total > 0) {

          if (total > this.pots[i].volume) {  // остаток больше текущей банки
            this.pots[i].filling = this.pots[i].volume;
            total = total - this.pots[i].volume; // остаток уменьшаем на объем банки

          } else {  // остаток меньше текущей банки
            this.pots[i].filling = total;
            total = 0;
          }
          this.pots[i].fruit = jam.fruit; // сохраняем тип варенья
          this.pots.splice(i, 1, this.pots[i]); // splice для реактивности
        }
      }
    }
  }
}
</script>

<template>
  <div id="app">
    <div class="ui container">
      <div class="ui equal width stackable grid">
        <Pot :pots="pots" :freeVolume="freeVolume" v-on:newPotAdded="newPotAdded" class="column"/>
        <Jam :fruits="fruits" :jams.sync="jams" v-on:newJamAdded="newJamAdded" class="column"/>
        <Storage :pots="pots" :fruits="fruits" class="column"/>
      </div>
    </div>
  </div>
</template>

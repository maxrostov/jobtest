<template>
  <div id="jam">
    <h3>Производство варенья</h3>
    <form class="ui form" v-on:submit.prevent="formSubmit">

      <div class="two fields">
        <div class="field">
          <label>Масса, г</label>
          <input v-model="newVolume" type="number">
        </div>
        <div class="field">
          <label style="color: transparent">-</label>
          <button class="ui tiny submit button">Добавить</button>
        </div>
      </div>

      <div class="inline fields">
        <div class="field" v-for="fruit in fruits" :key="fruit">
          <div class="ui radio checkbox">
            <input :id="fruit" type="radio" name="fruit_radio" required v-model="newFruit" v-bind:value="fruit">
            <label :for="fruit">{{ fruit }}</label>
          </div>
        </div>
      </div>
    </form>

    <div v-if="jams.length" class="ui basic segments">
      <div v-for="(jam, index) in jams" :key="index" class="ui segment">
        <span class="ui small label">{{ jam.fruit }}</span> {{ jam.volume }} грамм.
      </div>
    </div>
    <span v-else style="color: grey;">Пусто. Добавьте варенье.</span>

  </div>
</template>

<script>
export default {
  name: 'Jam',
  props: ['fruits', 'jams'],
  created() {
    this.newFruit = this.fruits[0];
  },
  data() {
    return {
      newVolume: 500,
      newFruit: '',
    }
  },
  methods: {
    formSubmit() {
      this.$emit('newJamAdded', {fruit: this.newFruit, volume: this.newVolume});
    }
  }
}
</script>

<style>
#jam {
  background: #edebe9;
}

</style>

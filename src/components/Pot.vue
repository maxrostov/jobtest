<template>
  <div id="pot">
    <h3>Пустые банки</h3>

    <form class="ui form" v-on:submit.prevent="formSubmit">
      <div class="three fields">
        <div class="field">
          <label>Подпись</label>
          <input v-model="newLabel" required autofocus placeholder="Подпись">
        </div>
        <div class="field">
          <label>Масса, г</label>
          <input v-model.number="newVolume" type="number">
        </div>
        <div class="field">
          <label style="color: transparent">-</label>
          <button class="ui tiny submit button">Добавить</button>
        </div>
      </div>
    </form>

    <br>
    <div v-if="emptyPots.length">
      <div v-for="(pot, index) in emptyPots"  :key="index" class="ui raised segment">
        Банка {{ pot.label }} ({{ pot.volume }} г)</div>
      <br>  Итого свободно: {{freeVolume}} г
    </div>
    <span v-else style="color: grey;">Пусто. Добавьте банку.</span>
  </div>
</template>

<script>
export default {
  name: 'Pot',
props:['pots','freeVolume'],

  data() {
    return {
      newLabel: '',
      newVolume: 200,
    }
  },
computed:{
    // фильтруем только пустые длявывода (потому что через props приходят и уже заполненые)
    emptyPots(){
      return this.pots.filter( (i) => i.filling == undefined )
    }
},
  methods: {
    formSubmit() {
         this.$emit('newPotAdded', {label:this.newLabel, volume:this.newVolume});

         // сбрасываем значения в форме добавления
      this.newLabel = '';
      this.newVolume = 200;
    }
  }
}
</script>

<style>
#pot{
  background: #fff;
}
</style>

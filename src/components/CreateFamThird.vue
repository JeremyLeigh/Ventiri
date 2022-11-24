<template>
    <div class="container add-form">
    <div>
    <div class="row fullWidth">
       <div class="col-4">
          <button @click="toPrev">&lsaquo; Go Back</button>
        </div>
       <div class="col-4">
          <h2>Attributes</h2>
       </div>
        </div>
    </div>
    <form @submit="toProf" class="add-form container">
        <div class="row fullWidth">
          <div class="col-4">
            <label for="str">Strength:</label><br>
            <input class="formRow" type="number" name="str" required v-model="str" max="50" min="1">
          </div>
            <div class="col-4">
            <label for="dex">Dexterity:</label><br>
            <input class="formRow" type="number" name="dex" required v-model="dex" max="50" min="1">
          </div>
            <div class="col-4">
            <label for="con">Constitution:</label><br>
            <input class="formRow" type="number" name="con" required v-model="con" max="50" min="1">
          </div>
        </div>
        <div class="row fullWidth">
          <div class="col-4">
            <label for="int">Intelligence:</label><br>
            <input class="formRow" type="number" name="int" required v-model="int" max="50" min="1">
          </div>
            <div class="col-4">
            <label for="wis">Wisdom:</label><br>
            <input class="formRow" type="number" name="wis" required v-model="wis" max="50" min="1">
          </div>
            <div class="col-4">
            <label for="cha">Charisma:</label><br>
            <input class="formRow" type="number" name="cha" required v-model="cha" max="50" min="1">
          </div>
        </div>

      <div class="row">
        <div class="col-4">
        <input class="submit" type="submit" value="Next">
        </div>
      </div>
    </form>
 </div>
</template>

<script>
  export default {
      name: 'createFamThird',
      data() {
        return {
          str: '',
          dex: '',
          con: '',
          int: '',
          wis: '',
          cha: ''
        }
      },
      methods: {
        toProf(e) {
          e.preventDefault()

          const newFamInfo = {
            str: this.str,
            dex: this.dex,
            con: this.con,
            int: this.int,
            wis: this.wis,
            cha: this.cha,
       
          }
          sessionStorage.setItem('famStat', JSON.stringify(newFamInfo));
          const statStr = sessionStorage.getItem('famStat');
          const famStat = JSON.parse(statStr);
          //console.log(famStat);
          const passPercCalc = Math.floor((parseInt(newFamInfo.wis) * 5 - 50) / 10) + 10;
          this.$emit('pass-Perc', passPercCalc);
          this.$emit('to-next', passPercCalc);
        },
          toPrev()
        {
          this.$emit('to-prev');
        },
           applyStatus()
        {
          //console.log("data up to date");
          //Assignment
          this.str = this.stat['str'];
          this.dex = this.stat['dex'];
          this.con = this.stat['con'];
          this.int = this.stat['int'];
          this.wis = this.stat['wis'];
          this.cha = this.stat['cha'];
        }
      },
      props:{
        stat: Object,
        speeds: Array,
        traits: Array,
        senses: Array,
        actions: Array,
        skills: Array,
        rulesState: Boolean,
        loadState: Boolean
      }
  }
</script>

<style scoped>
 .add-form div
 {
  padding: 10px;
 }
 .formRow
 {
  margin-top: 5px;
 }
.fullWidth
{
    width: 100%;
    margin-right: 0;
    margin-left: 0;
}
 input, select
 {
  width: 100%;
  padding: 5px;
 }
input
{
height: 40px;
}

 label
 {
  font-weight: bold;
 }
</style>
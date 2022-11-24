<template>
    <div class="container add-form">
    <div>
    <div class="row fullWidth">
       <div class="col-4">
          <button @click="toPrev">&lsaquo; Go Back</button>
        </div>
       <div class="col-4">
          <h2>Survival Details</h2>
       </div>
        </div>
    </div>
    <form @submit="toStats" class="add-form container">
        <div class="row fullWidth">
          <div class="col-6">
            <label for="ac">Armor Class:</label><br>
            <input class="formRow" type="number" name="ac" required v-model="ac" max="50" min="1">
            <div class="checkboxWrap skinnyBox formRow">
            <label for="nat">Modified AC</label>
            <input @click="enableMod(); natState = !natState" class="checkbox" type="checkbox" name="nat" value="true" v-model="nat">
            <input ref="acMod" class="formRow" type="number" name="modAc" v-model="modAc" max="50" min="1" disabled="true">
            <input ref="modDesc" class="formRow" type="text" name="modDesc" v-model="modDesc" max="50" min="1" disabled="true" placeholder="(e.g. Leather Armor)">
            </div>
          </div>
          <div class="col-6">
            <div class="skinnyBox hpBox">
            <label for="hp">Hit Points:</label><br>
            <input class="col-4 formRow" type="number" required name="hp" v-model="hp" max="1000" min="1">
            <input class="col-4 formRow" type="number" required name="hitNum" v-model="hitNum" max="100" min="0">
            <select class="col-4 formRow" v-model="hitDice" name="hitDice" required>
                <option value="" disabled selected hidden> - </option>
                <option value="d4">d4</option>
                <option value="d6">d6</option>
                <option value="d8">d8</option>
                <option value="d10">d10</option>
                <option value="d12">d12</option>
            </select>
            </div>
          </div>
        </div>
       <div class="topRow">
        <label>Speeds</label>
        <button @click="addSpeed">Add Speed</button>
        </div>
        <div class="row fullWidth">
          <div v-for="(speed, counter) in speeds" v-bind:key="counter" class="genBox skinnyBox col-6">
            <div class="innerBox skinnyBox">
            <span @click="deleteSpeed(counter)">&#10005;</span>
            <div class="skinnyBox skillRow">
            <div class="col-8">
            <select name="speedName" required v-model="speeds[counter].speedType" max="50" min="1">
                <option value="" disabled selected hidden> - </option>
                <option value="Walking">Walking</option>
                <option value="Flying">Flying</option>
                <option value="Hovering">Hovering</option>
                <option value="Burrowing">Burrowing</option>
                <option value="Climbing">Climbing</option>
                <option value="Swimming">Swimming</option>
            </select>
            </div>
            <div class="col-4">
            <input type="number" name="speedValue" required v-model="speeds[counter].speedValue" min="1">
            </div>
            </div>
          </div>
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
      name: 'createFamSecond',
      emits: ["to-next","to-prev"],
      props: {
        stat: Object,
        speed: Array,
        dataState: Boolean,
      },
      data() {
        return {
          ac: '',
          nat: false,
          modAc: '',
          modDesc: '',
          hp: '',
          hitNum: '',
          hitDice: '',
          speeds: [],
          natState: false,
        }
      },
      methods: {
        toStats(e) {
          e.preventDefault()

          const newFamInfo = {
            "ac-num": this.ac,
            "is-nat": this.nat,
            "ac-mod": this.modAc,
            "ac-desc": this.modDesc,
            "max-hp": this.hp,
            "hit-dice-num": this.hitNum,
            "hit-dice-type": this.hitDice
          }
            for (let i = 0; i < this.speeds.length; i++) {
            const fullSpeed = this.speeds[i].speedType + ` ` + this.speeds[i].speedValue + ` Ft`;
            //console.log(fullSpeed);
            newFamInfo['speed-' + (i+1)] = fullSpeed;
          }
          sessionStorage.setItem('famInfo', JSON.stringify(newFamInfo));
          const infoStr = sessionStorage.getItem('famInfo');
          const info = JSON.parse(infoStr);
          //console.log(info);
          this.$emit('to-next');
        },
          toPrev()
        {
          this.$emit('to-prev');
        },
        addSpeed(){
          if (this.speeds.length <= 5)
          {
          this.speeds.push({
            speedType: '',
            speedValue: ''
          });
          }
           else
          {
            alert("Maximum Six Speeds");
          }
        },
      deleteSpeed(counter){
      this.speeds.splice(counter,1);
      },
      enableMod()
      {
      if (this.nat == false)
      {
        //console.log(this.$refs.acMod);
        this.$refs.acMod.disabled = false;
        this.$refs.modDesc.disabled = false;
      }
      else{
        //console.log(this.$refs.acMod);
        this.$refs.acMod.disabled = true;
        this.$refs.modDesc.disabled = true;
      }
      },
      applyStatus()
        {
          //console.log("data up to date");
          //Assignment
            this.ac = this.stat['ac-num'];
            this.nat = this.stat['is-nat'];
            this.modAc = this.stat['ac-mod'];
            this.modDesc = this.stat['ac-desc'];
            this.hp = this.stat['max-hp'];
            this.hitNum = this.stat['hit-dice-num'];
            this.hitDice = this.stat['hit-dice-type'];
            //Speeds
            //console.log(this.speeds);
            for (let i = 0; i < this.speed.length; i++) {
              let speedString = this.speed[i];
              let cutString = speedString.split(" ");
              //console.log(cutString[0],cutString[1]);
              this.speeds.push({
              speedType: cutString[0],
              speedValue: cutString[1]
              });
            }
          }
        },
      }
</script>

<style scoped>
 .add-form div:not(.skinnyBox)
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
  height: 40px;
 }

 label
 {
  font-weight: bold;
 }
 .genBox
{
  display: flex;
  flex-direction: column;
  padding:10px;
}
.skillRow
{
  display: inline-flex;
   width: 100%;
}

.innerBox
{
  border-width: 2px;
  border-color: #A59757;
  border-style: solid;
}

.topRow
{
  display: inline-flex;
  width: 100%;
}
.topRow button
{
 position: absolute;
 right:30px;
}
.checkbox
{
  width: 20px;
  height: 20px;
  margin-left: 10px;
  position: relative;
  bottom: -4px;
}
.checkboxWrap
{
  margin: 0;
  padding-top: 20px;
}

.hpBox
{
  padding-top: 0;
}
</style>
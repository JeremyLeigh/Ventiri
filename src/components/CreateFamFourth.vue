<template>
    <div class="container add-form">
    <div>
    <div class="row fullWidth">
       <div class="col-4">
          <button @click="toPrev">&lsaquo; Go Back</button>
        </div>
       <div class="col-4">
          <h2>Senses &#38; Skills</h2>
       </div>
        </div>
    </div>
    <form @submit="toActions" class="add-form container">
        <div class="row fullWidth">
          <div class="col-4">
            <label for="passP">Passive Perception:</label><br>
            <input class="formRow" type="number" name="passP" required v-model="passP" max="50" min="1" :placeholder="[[passPerc]]">
          </div>
        </div>
        <div class="topRow">
        <label>Senses</label>
        <button @click="addSense">Add Sense</button>
        </div>
          <div class="row fullWidth">
          <div v-for="(sense, counter) in senses" v-bind:key="counter" class="genBox skinnyBox col-6">
            <div class="innerBox skinnyBox">
            <span @click="deleteSense(counter)">&#10005;</span>
            <div class="skinnyBox skillRow">
            <div class="col-8">
            <select name="senseType" required v-model="senses[counter].senseType" max="50" min="1">
                <option value="" disabled selected hidden> - </option>
                <option value="Blindsight">Blindsight</option>
                <option value="Darkvision">Darkvision</option>
                <option value="Tremorsense">Tremorsense</option>
                <option value="Truesight">Truesight</option>
            </select>
            </div>
            <div class="col-4">
            <input type="number" name="senseValue" required v-model="senses[counter].senseValue" min="1">
            </div>
            </div>
          </div>
          </div>
        </div>

        <div class="topRow">
        <label>Skills</label>
        <button @click="addSkill">Add Skill</button>
        </div>
        <div class="row fullWidth">
          <div v-for="(skill, counter) in skills" v-bind:key="counter" class="genBox skinnyBox col-6">
            <div class="innerBox skinnyBox">
            <span @click="deleteSkill(counter)">&#10005;</span>
            <div class="skinnyBox skillRow">
            <div class="col-8">
            <select name="skillType" required v-model="skills[counter].skillType" max="50" min="1">
                <option value="" disabled selected hidden> - </option>
                <option value="Acrobatics">Acrobatics</option>
                <option value="Animal Handling">Animal Handling</option>
                <option value="Arcana">Arcana</option>
                <option value="Athletics">Athletics</option>
                <option value="Deception">Deception</option>
                <option value="History">History</option>
                <option value="Insight">Insight</option>
                <option value="Intimidation">Intimidation</option>
                <option value="Investigation">Investigation</option>
                <option value="Medicine">Medicine</option>
                <option value="Nature">Nature</option>
                <option value="Perception">Perception</option>
                <option value="Performance">Performance</option>
                <option value="Persuasion">Persuasion</option>
                <option value="Religion">Religion</option>
                <option value="Sleight of Hand">Slieght of Hand</option>
                <option value="Stealth">Stealth</option>
                <option value="Survival">Survival</option>
            </select>
            </div>
            <div class="col-4">
            <input type="number" name="skillValue" required v-model="skills[counter].skillValue" max="50" min="1">
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
      name: 'createFamFourth',
       props:{
        sense: Array,
        skill: Array,
        stat: Object,
        passPerc: {
          type: Number,
          default: 0,
        }
       },
      data() {
        return {
          passP: '',
          senses: [],
          skills: []
        }
      },
      methods: {
        toActions(e) {
          e.preventDefault()

          const newFamInfo = {
            'passive-perc': this.passP
          }

          for (let i = 0; i < this.senses.length; i++) {
            const fullSense = this.senses[i].senseType + ` ` + this.senses[i].senseValue + ` Ft`;
            //console.log(fullSense);
            newFamInfo['sense-' + (i+1)] = fullSense;
          }
           for (let i = 0; i < this.skills.length; i++) {
            const fullSkill = this.skills[i].skillType + ` +` + this.skills[i].skillValue;
            //console.log(fullSense);
            newFamInfo['skill-' + (i+1)] = fullSkill;
          }

          sessionStorage.setItem('famProf', JSON.stringify(newFamInfo));
          const profStr = sessionStorage.getItem('famProf');
          const famProf = JSON.parse(profStr);
          //console.log(famProf);
        this.$emit('to-next');
        },
          toPrev()
        {
          this.$emit('to-prev');
        },
        addSkill(){
          if (this.skills.length <= 8)
          {
          this.skills.push({
            skillType: '',
            skillValue: ''
          });
          }
           else
          {
            alert("Maximum Nine Skills");
          }
        },
      deleteSkill(counter){
      this.skills.splice(counter,1);
      },
          addSense(){
          if (this.senses.length <= 4)
          {
          this.senses.push({
            senseType: '',
            senseValue: ''
          });
          }
           else
          {
            alert("Maximum Five Senses");
          }
        },
      deleteSense(counter){
      this.senses.splice(counter,1);
      },
            applyStatus()
        {
          //console.log("data up to date");
          //Assignment
            this.passP = this.stat['passive-perc'];
          //Senses
            for (let i = 0; i < this.sense.length; i++) {
            let senseString = this.sense[i];
            let cutSense = senseString.split(" ");
            this.senses.push({
            senseType: cutSense[0],
            senseValue: cutSense[1]
            });
            }
          //Skills
            for (let i = 0; i < this.skill.length; i++) {
            let skillString = this.skill[i];
            let cutSkill = skillString.split(" ");
            //console.log(cutSkill[0], cutSkill[1]);
            this.skills.push({
            skillType: cutSkill[0],
            skillValue: cutSkill[1].substring(1)
            });
            }
          }
      }
  }
</script>

<style>
 .add-form
 {
  padding: 10px;
  border-width: 2px;
  border-color: #A59757;
  border-style: solid;
 } 
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
  height: 40px;
 }
input
{
height: 40px;
}

 label
 {
  font-weight: bold;
 }

textarea
{
  width: 100%;
  height: 100px;
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

</style>
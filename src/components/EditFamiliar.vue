<template>
    <CreateFamBasic ref="basic" :loadState="loadState" :inEdit="inEdit" :class="{ 'hide': hideList[0]}" @remove-loader="$emit('remove-loader')" @to-next="nextField" :stat="stat" />
    <CreateFamSecond ref="second" :inEdit="inEdit" :class="{ 'hide': hideList[1]}" @to-next="nextField" @to-prev="prevField" :stat="stat" :speed="speeds" />
    <CreateFamThird ref="third" :inEdit="inEdit" :class="{ 'hide': hideList[2]}" @pass-Perc="definePassP" @to-next="nextField" @to-prev="prevField" :stat="stat" />
    <CreateFamFourth ref="fourth" :inEdit="inEdit" :passPerc="passPerc" :class="{ 'hide': hideList[3]}" @to-next="nextField" @to-prev="prevField" :stat="stat" :sense="senses" :skill="skills" />
    <CreateFamFifth ref="fifth" :inEdit="inEdit" :class="{ 'hide': hideList[4]}" @to-next="nextField" @to-prev="prevField" :trait="traits" />
    <CreateFamSixth ref="sixth" :inEdit="inEdit" :class="{ 'hide': hideList[5]}" @to-next="nextField" @to-prev="prevField" :action="actions" />
    <div class="circle" ></div>
</template>

<script>
import CreateFamBasic from './CreateFamBasic.vue'
import CreateFamSecond from './CreateFamSecond.vue'
import CreateFamThird from './CreateFamThird.vue'
import CreateFamFourth from './CreateFamFourth.vue'
import CreateFamFifth from './CreateFamFifth.vue'
import CreateFamSixth from './CreateFamSixth.vue'

  export default {
  components: {
      CreateFamBasic,
      CreateFamSecond,
      CreateFamThird,
      CreateFamFourth,
      CreateFamFifth,
      CreateFamSixth,
      },
      name: 'EditFamiliar',
      emits: ["postfam", "remove-loader", 'add-loader'],
      props:{
        stat: Object,
        speeds: Array,
        traits: Array,
        senses: Array,
        actions: Array,
        skills: Array,
        dataState: Boolean,
        loadState: Boolean,
      },
      data()
      {
        return {
          hideList : [
          false,
          true,
          true,
          true,
          true,
          true
          ],
          passPerc: 0,
          currentForm: 0,
          inEdit: true,
        }
      },
      methods: {
        nextField() {
          for (let i = 0; i < this.hideList.length; i++) {
            this.hideList[i] = true;
          }
          let nextForm = this.currentForm + 1;
          this.hideList[nextForm] = false;
          if (this.currentForm == 5)
          {
            this.readAll();
          }
           this.currentForm++
           //console.log(this.currentForm);
          },
        definePassP(passPercCalc)
        {
          this.passPerc = passPercCalc;
        },
        prevField() {
          for (let i = 0; i < this.hideList.length; i++) {
            this.hideList[i] = true;
          }
          this.currentForm--
          this.hideList[this.currentForm] = false;
          //console.log(this.currentForm);
        },
        readAll() {
          const basicDetsStr = sessionStorage.getItem('basicDets');
          const infoStr = sessionStorage.getItem('famInfo');
          const statStr = sessionStorage.getItem('famStat');
          const profStr = sessionStorage.getItem('famProf');
          const traitStr = sessionStorage.getItem('famTrait');
          const actionStr = sessionStorage.getItem('famAction');
          const basicDets = JSON.parse(basicDetsStr);
          const info = JSON.parse(infoStr);
          const stats = JSON.parse(statStr);
          const profs = JSON.parse(profStr);
          const traits = JSON.parse(traitStr);
          const actions = JSON.parse(actionStr);
          const newFam = {
          ...basicDets,
          ...info,
          ...stats,
          ...profs,
          ...traits,
          ...actions
          };
          console.log(newFam);
          this.$emit('postfam', newFam);
        },
        uploadStatus(confirmCode)
        {
        if (confirmCode === 200) {
          alert("Upload Confirmed");
        }
        else {
          alert("Error 500");
        }
        },
        statusCheck()
        {
          if (this.$refs.basic != undefined)
        {
          setTimeout(() => {
        console.log("Definded and Changing Fields");
        this.currentForm = 0;
        for (let i = 0; i < this.hideList.length; i++) {
            this.hideList[i] = true;
          }
        this.hideList[0] = false;
        this.$refs.basic.applyStatus();
        this.$refs.second.applyStatus();
        this.$refs.third.applyStatus();
        this.$refs.fourth.applyStatus();
        this.$refs.fifth.applyStatus();
        this.$refs.sixth.applyStatus();
          }, 1000);
        }
        else
        {
        console.log("Edit Fam is checking again...");
        setTimeout(() => {
          this.statusCheck();
        }, 1000);
        }
        },
  },
   async created() {
      //console.log("Edit Created");
      this.$emit('add-loader');
      this.statusCheck();
    },
  }
</script>

<style>
.show
{
  display: block;
}
.hide
{
  display: none;
}

.genBox span
{
  position: absolute;
  z-index: 2;
  right: 8px;
  top: 8px;
  font-size: 15px;
  height: 0;
  color: #A59757;
  cursor: pointer;
}

.skinnyBox
{
  padding: 0;
  padding-top: 20px;
}

</style>
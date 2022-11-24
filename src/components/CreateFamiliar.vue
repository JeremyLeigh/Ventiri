<template>
  <div class="maxWidth">
    <CreateFamBasic class="maxWidth" :inEdit="inEdit" :class="{ 'hide': hideList[0]}" @to-next="nextField" />
    <CreateFamSecond class="maxWidth" :class="{ 'hide': hideList[1]}" @to-next="nextField" @to-prev="prevField" />
    <CreateFamThird class="maxWidth" :class="{ 'hide': hideList[2]}" @pass-Perc="definePassP" @to-next="nextField" @to-prev="prevField" />
    <CreateFamFourth class="maxWidth" :passPerc="passPerc" :class="{ 'hide': hideList[3]}" @to-next="nextField" @to-prev="prevField" />
    <CreateFamFifth class="maxWidth" :class="{ 'hide': hideList[4]}" @to-next="nextField" @to-prev="prevField" />
    <CreateFamSixth class="maxWidth" :class="{ 'hide': hideList[5]}" @to-next="nextField" @to-prev="prevField" />
    <div class="circle" ></div>
  </div>
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
      name: 'CreateFamiliar',
      prop:{
        dataState: Boolean,
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
          inEdit: false,
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
           console.log(this.currentForm);
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
          console.log(this.currentForm);
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
          this.$emit('post-fam', newFam);
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
        selectMode()
        {
          console.log("MODE SELECTED");
        },
  },
   async created() {
      //console.log("Create Created");
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
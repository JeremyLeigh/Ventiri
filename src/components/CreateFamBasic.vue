<template>
    <div class="add-form container">
    <div>
    <div class="row fullWidth">
       <div class="centerMe">
          <h2>Creature Details</h2>
       </div>
        </div>
      </div>
     <div class="centerMe">
          <div :class="{ 'showLoader': loadState}" class="loader"><Loader /></div>
      </div>
    <form @submit="toNext" :class="{ 'hideContent': loadState}" class="add-form container">
        <div class="row">
          <div class="col-8">
            <label for="uName">Unique Name:</label><br>
            <input class="formRow" type="text" name="uName" v-model="uName" maxlength="18">
          </div>
          <div class="col-4">
            <label for="cName">Creature Name:</label><br>
            <input class="formRow" type="text" required name="cName" v-model="cName" placeholder= "(e.g. Frog)">
          </div>
        </div>
        <div class="row">
        <div class="col-4">
          <label for="size">Size:</label><br>
          <select class="formRow" v-model="size" required name="size">
            <option value="" disabled selected hidden> - </option>
            <option value="Tiny">Tiny</option>
            <option value="Small">Small</option>
            <option value="Medium">Medium</option>
            <option value="Large">Large</option>
            <option value="Huge">Huge</option>
            <option value="Gargantuan">Gargantuan</option>
          </select>
        </div>
        <div class="col-4">
          <label for="cType">Creature Type:</label><br>
          <select class="formRow" v-model="cType" required name="cType">
            <option value="" disabled selected hidden> - </option>
            <option value="Aberration">Aberration</option>
            <option value="Beast">Beast</option>
            <option value="Celestial">Celestial</option>
            <option value="Construct">Construct</option>
            <option value="Dragon">Dragon</option>
            <option value="Elemental">Elemental</option>
            <option value="Fey">Fey</option>
            <option value="Fiend">Fiend</option>
            <option value="Giant">Giant</option>
            <option value="Humanoid">Humanoid</option>
            <option value="Monstrosity">Monstrosity</option>
            <option value="Ooze">Ooze</option>
            <option value="Plant">Plant</option>
            <option value="Beast Swarm">Beast Swarm</option>
            <option value="Undead">Undead</option>
            <option value="Undefined">Undefined</option>
          </select>
        </div>
        <div class="col-4">
          <label for="align1">Alignment:</label><br>
          <select class="formRow" v-model="align1" name="align1">
            <option value="" disabled selected hidden> - </option>
            <option value="Lawful">Lawful</option>
            <option value="Neutral">Neutral</option>
            <option value="Chaotic">Chaotic</option>
          </select>
          <select class="formRow" v-model="align2" name="align2">
            <option value="" disabled selected hidden> - </option>
            <option value="Good">Good</option>
            <option value="Neutral">Neutral</option>
            <option value="Evil">Evil</option>
          </select>
        </div>
      </div>
      <div class="row textRow">
        <label for="desc">Creature Description</label>
        <textarea class="col-12 formRow" name="desc" v-model="desc"></textarea>
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
import Loader from './Loader.vue';
  export default {
      name: 'CreateFamBasic',
      data() {
        return {
          uName: '',
          cName: '',
          size: '',
          cType: '',
          align1: '',
          align2: '',
          desc: ''
        }
      },
      emits: ["remove-loader"],
      components: {
        Loader,
      },
      props:{
        stat: Object,
        inEdit: Boolean,
        loadState: Boolean,
      },
      methods: {
        toNext(e) {
          e.preventDefault()

          const newFam = {
            "unique-name" : this.uName,
            "creature-name" : this.cName,
            "size" : this.size,
            "creature-type" : this.cType,
            "alignment" : this.align1 +` `+ this.align2,
            "description" : this.desc,
          }
          sessionStorage.setItem('basicDets', JSON.stringify(newFam));
          const basicDetsStr = sessionStorage.getItem('basicDets');
          const basicDets = JSON.parse(basicDetsStr);
          //console.log(basicDets);
          this.$emit('to-next');
        },
          applyStatus()
        {
          //Assignment
          this.uName = this.stat['unique-name'];
          this.cName = this.stat['creature-name'];
          this.size = this.stat['size'];
          this.cType = this.stat['creature-type'];
          let alignText = this.stat['alignment'];
          //Alignment Split
          const alignArray = alignText.split(" ");
          //Assignment Cont
          this.align1 = alignArray[0];
          this.align2 = alignArray[1];
          this.desc = this.stat['description'];
          console.log("data up to date");
          console.log(this.stat);
          this.$emit('remove-loader');
        },
         toPrev()
        {
          this.$emit('to-prev');
        }
      },
       
  }
</script>

<style scoped>
.centerMe
{
  display: flex;
  align-content: center;
  justify-content: center;
  width:100%;
}
 .add-form div
 {
  padding: 10px;
 }
 .formRow
 {
  margin-top: 5px;
 }
 input, select
 {
  width: 100%;
  padding: 5px;
 }
 .fullWidth
{
    width: 100%;
     margin-right: 0;
    margin-left: 0;
}
input
{
height: 40px;
}

 label
 {
  font-weight: bold;
 }
 .textRow
 {
  margin:0;
 }
 .textRow label
 {
  padding:0;
 }

 .loader
{
  margin: 0 auto;
  z-index: 10;
  display: none;
  align-items: center;
  justify-content: center;
  width:100%;
  top: 150px;
}
.showLoader
{
  display: flex;
}

.hideContent
{
  opacity: 0;
}
</style>
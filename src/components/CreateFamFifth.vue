<template>
  <div class="container add-form">
    <div>
    <div class="row fullWidth">
       <div class="col-4">
          <button @click="toPrev">&lsaquo; Go Back</button>
        </div>
       <div class="col-4">
          <h2>Traits</h2>
       </div>
        </div>
    </div>
    <form @submit="toActions" class="add-form container">
        <button @click="addTrait">Add Trait</button>
        <div class="row fullWidth">
          <div v-for="(trait, counter) in traits" v-bind:key="counter" class="genBox">
            <div class="innerBox">
            <span @click="deleteTrait(counter)">&#10005;</span><br>
            <div class="col-4">
            <label for="traitName">{{counter+1}}. Trait Name</label><br>
            <input class="formRow" type="text" name="traitName" required v-model="traits[counter].traitName" max="50" min="1">
            </div>
            <div class="col-12">
            <label for="traitDesc">Trait Desc.</label><br>
            <textarea class="formRow" name="traitDesc" required v-model="traits[counter].traitDesc" max="800" min="1"></textarea>
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
      name: 'createFamFifth',
      props: {
        trait: Array
      },
      data() {
        return {
          traits: []
        }
      },
      methods: {
        toActions(e) {
          e.preventDefault()

          const newFamInfo = {};

            for (let i = 0; i < this.traits.length; i++) {
            const fulltrait = this.traits[i].traitName + `. ` + this.traits[i].traitDesc;
            //console.log(fulltrait);
            newFamInfo['trait-' + (i+1)] = fulltrait;
          }

          sessionStorage.setItem('famTrait', JSON.stringify(newFamInfo));
          const traitStr = sessionStorage.getItem('famTrait');
          const famTrait = JSON.parse(traitStr);
          //console.log(famTrait);
          this.$emit('to-next');
        },
           toPrev()
        {
          this.$emit('to-prev');
        },
        addTrait(){
          if (this.traits.length <= 5)
          {
          this.traits.push({
            traitName: '',
            traitDesc: ''
          });
          }
           else
          {
            alert("Maximum Six Traits");
          }
        },
      deleteTrait(counter){
      this.traits.splice(counter,1);
      },
       applyStatus()
        {
          //console.log("data up to date");
          //Assignment
          //Traits
            for (let i = 0; i < this.trait.length; i++) {
            const [name, ...rest] = this.trait[i].split('.');
            const remainder = rest.join('.');
            this.traits.push({
            traitName: name,
            traitDesc: remainder
            });
            }
        }
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

button
{
  margin-top: 50px;
}

textarea
{
  width: 100%;
  height: 100px;
}
.innerBox
{
  border-width: 2px;
  border-color: #A59757;
  border-style: solid;
}

</style>
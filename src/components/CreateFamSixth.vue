<template>
   <div class="container add-form">
    <div>
    <div class="row fullWidth">
       <div class="col-4">
          <button @click="toPrev">&lsaquo; Go Back</button>
        </div>
       <div class="col-4">
          <h2>Actions</h2>
       </div>
        </div>
    </div>
    <form @submit="toNext" class="add-form container">
        <button @click="addAction">Add Action</button>
        <div class="row fullWidth">
          <div v-for="(action, counter) in actions" v-bind:key="counter" class="genBox">
            <label for="actionName">{{counter+1}}. Action Name</label>
            <span @click="deleteAction(counter)">&#10005;</span>
            <div class="col-12 skillRow">
            <div class="col-4">
                <input type="text" name="actionName" required v-model="actions[counter].actionName" max="50" min="1">   
            </div>
            <div class="col-4">
            <select name="actionType" required v-model="actions[counter].actionType">
                <option value="" disabled selected hidden> - </option>
                <option value="action">Action</option>
                <option value="b-action">Bonus Action</option>
                <option value="reaction">Reaction</option>
                <option value="no-action">No Action</option>
            </select>
            </div>
            </div>
            <div class="col-12">
            <label for="actionDesc">Action Desc.</label><br>
            <textarea class="formRow" name="actionDesc" required v-model="actions[counter].actionDesc" max="500" min="1"></textarea>
            </div>
          </div>
        </div>

      <div class="row">
        <div class="col-4">
        <input class="submit" type="submit" value="Submit">
        </div>
      </div>
    </form>
    </div>
</template>

<script>
  export default {
      name: 'createFamSixth',
      props: {
        action: Array
      },
      data() {
        return {
          actions: []
        }
      },
      methods: {
        toNext(e) {
          e.preventDefault()

          const newFamInfo = {};
            let aCounter = 0;
            let baCounter = 0;
            let rCounter = 0;
            let naCounter = 0;
            for (let i = 0; i < this.actions.length; i++) {
            const fullAction = this.actions[i].actionType + `/*/` + this.actions[i].actionName + `. ` + this.actions[i].actionDesc;
            //console.log(fullAction);
            if (this.actions[i].actionType == 'action') {
                //console.log("This is an action -> " + fullAction);
                aCounter++;
                newFamInfo[this.actions[i].actionType + `-` + (aCounter)] = fullAction;
            }
             if (this.actions[i].actionType == 'b-action') {
                //console.log("This is a bonus action -> " + fullAction);
                baCounter++;
                newFamInfo[this.actions[i].actionType + `-` + (baCounter)] = fullAction;
            }
             if (this.actions[i].actionType == 'reaction') {
                //console.log("This is a reaction -> " + fullAction);
                rCounter++;
                newFamInfo[this.actions[i].actionType + `-` + (rCounter)] = fullAction;
            }
             if (this.actions[i].actionType == 'no-action') {
                //console.log("This is a no action -> " + fullAction);
                naCounter++;
                newFamInfo[this.actions[i].actionType + `-` + (naCounter)] = fullAction;
            }
          }
          sessionStorage.setItem('famAction', JSON.stringify(newFamInfo));
          const actionStr = sessionStorage.getItem('famAction');
          const famAction = JSON.parse(actionStr);
          //console.log(famAction);
          this.$emit('to-next');
        },
           toPrev()
        {
          this.$emit('to-prev');
        },
        addAction(){
          if (this.actions.length <= 10)
          {
          this.actions.push({
            actionName: '',
            actionType: '',
            actionDesc: ''
          });
          }
           else
          {
            alert("Maximum Eleven Actions");
          }
        },
      deleteAction(counter){
      this.actions.splice(counter,1);
      },
       applyStatus()
        {
          //console.log(this.action);
          //Assignment
          //Actions
            for (let i = 0; i < this.action.length; i++) {
            const [type, ...details] = this.action[i].split('/*/');
            //console.log(type);
            //console.log(details);
            const [name, ...rest] = details[0].split('.');
            const remainder = rest.join('.');
            //console.log(name);
            //console.log(remainder);
            this.actions.push({
            actionName: name,
            actionType: type,
            actionDesc: remainder
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
<template> 
  <div class="wrapper">
      <div :class="{ 'showLoader': loadState}" class="loader"><Loader /></div>
      <div :class="{ 'hideContent': loadState}" class="creature-wrapper">
        <div class="title-wrapper">
          <div class="first-half no-padding">
            <h2 class="bold">{{stat["unique-name"] != null ? stat["unique-name"] : stat["creature-name"]}}</h2>
            <h4>{{ stat["creature-name"]}}</h4>
            <p>{{ stat["size"]}} / {{ stat["creature-type"]}}, {{ stat["alignment"]}}</p>
          </div>
          <div class="second-half no-padding">
            <button @click="rulesState = !rulesState">{{rulesState === true? 'Show Rules' : 'Hide Rules'}}</button>
          </div>
        </div>
        <div :class="{ 'hideRules': rulesState}" class="full-length rules"><p :class="{ 'loadLate': rulesState}">You gain the service of a familiar, a spirit that takes an animal form you choose: bat, cat, crab, frog (toad), hawk, Lizard, Octopus, owl, Poisonous Snake, fish (quipper), rat, raven, Sea Horse, Spider, or Weasel. Appearing in an unoccupied space within range, the familiar has the Statistics of the chosen form, though it is a Celestial, fey, or fiend (your choice) instead of a beast.<br><br>
        Your familiar acts independently of you, but it always obeys your commands. In Combat, it rolls its own Initiative and acts on its own turn. A familiar can't Attack, but it can take other Actions as normal.<br><br>When the familiar drops to 0 Hit Points, it disappears, leaving behind no physical form. It reappears after you cast this spell again.<br><br>While your familiar is within 100 feet of you, you can communicate with it telepathically. Additionally, as an Action, you can see through your familiar's eyes and hear what it hears until the start of your next turn, gaining the benefits of any Special Senses that the familiar has. During this time, you are deaf and blind with regard to your own Senses.<br><br>As an Action, you can temporarily dismiss your familiar. It disappears into a pocket dimension where it awaits your summons. Alternatively, you can dismiss it forever. As an Action while it is temporarily dismissed, you can cause it to reappear in any unoccupied space within 30 feet of you.<br><br>You can't have more than one familiar at a time. If you cast this spell while you already have a familiar, you instead cause it to adopt a new form. Choose one of the forms from the above list. Your familiar transforms into the chosen creature.<br><br>Finally, when you Cast a Spell with a range of touch, your familiar can deliver the spell as if it had cast the spell. Your familiar must be within 100 feet of you, and it must use its Reaction to deliver the spell when you cast it. If the spell requires an Attack roll, you use your Attack modifier for the roll.</p></div>
        <hr>
        <div class="first-wrapper">
          <p><span class="bold">Armor Class&nbsp;:&nbsp;</span> {{ stat["ac-num"]}} {{ stat["is-nat"] === "true" ? "(Natural Armor)" : ""}} {{stat["ac-mod"] = !undefined ? stat["ac-mod"] : ""}} {{stat["ac-desc"] = !undefined ? (stat["ac-desc"]) : ""}}</p>
          <p><span class="bold">Hit Points&nbsp;:&nbsp;</span> {{ stat["max-hp"]}} ({{ stat["hit-dice-num"]}} {{ stat["hit-dice-type"]}})</p>
          <ul class="horiz">
            <p><span class="bold">Speed&nbsp;:&nbsp;</span> </p>
            <li :key="speed.id" v-for="speed in speeds"><p>{{speed}}&nbsp;</p></li>
          </ul>
        </div>
        <hr>
        <div class="stats-wrapper">
          <ul>
            <li>{{ stat["str"]}}<br>STR<div class="stat-mod"><p>{{ stat["str"] >10? '+' : ''}}{{ Math.floor((parseInt(stat["str"]) * 5 - 50) / 10) }}</p></div></li>
            <li>{{ stat["dex"]}}<br>DEX<div class="stat-mod"><p>{{ stat["dex"] >10? '+' : ''}}{{ Math.floor((parseInt(stat["dex"]) * 5 - 50) / 10) }}</p></div></li>
            <li>{{ stat["con"]}}<br>CON<div class="stat-mod"><p>{{ stat["con"] >10? '+' : ''}}{{ Math.floor((parseInt(stat["con"]) * 5 - 50) / 10) }}</p></div></li>
            <li>{{ stat["int"]}}<br>INT<div class="stat-mod"><p>{{ stat["int"] >10? '+' : ''}}{{ Math.floor((parseInt(stat["int"]) * 5 - 50) / 10) }}</p></div></li>
            <li>{{ stat["wis"]}}<br>WIS<div class="stat-mod"><p>{{ stat["wis"] >10? '+' : ''}}{{ Math.floor((parseInt(stat["wis"]) * 5 - 50) / 10) }}</p></div></li>
            <li>{{ stat["cha"]}}<br>CHA<div class="stat-mod"><p>{{ stat["cha"] >10? '+' : ''}}{{ Math.floor((parseInt(stat["cha"]) * 5 - 50) / 10) }}</p></div></li>
          </ul>
        </div>
        <hr>
        <div class="third-wrapper">
          <ul class="horiz goldBorder">
            <p class="gridLabel"><span class="bold">Skills&nbsp;:&nbsp;</span></p>
            <div class="skillGrid"><li :key="skill.id" v-for="skill in skills"><p>&nbsp;{{skill}}</p></li></div>
          </ul>
          <ul class="horiz goldBorder">
            <p class="gridLabel"><span class="bold">Senses&nbsp;:&nbsp;</span></p>
            <li><div class="senseGrid"><p :key="sense.id" v-for="sense in senses">{{sense}}</p></div></li>
          </ul>
          <p class="goldBorder"><span class="bold">Passive Perception&nbsp;:&nbsp;</span> {{stat["passive-perc"]}}</p>
        </div>
        <hr>
        <div class="trait-wrapper">
          <p class="bold">Traits</p>
          <ul>
            <li :key="trait.id" v-for="trait in traits"><p>{{trait}}</p></li>
          </ul>
        </div>
        <hr>
        <div class="actions-wrapper">
          <p class="bold">Actions</p>
          <ul>
            <li :key="actionData" v-for="actionData in actionDatas"><p>{{actionData}}</p></li>
            <p>{{actions.length < 1 ? "No Actions" : ""}}</p>
          </ul>
        </div>
        <DeleteFam class="deleteBar" :stat="stat" @delete-id="checkId" />
      </div>
    </div>
</template>

<script>
import Loader from './Loader.vue';
import DeleteFam from './DeleteFam.vue';
  export default {
      name: 'FindFamiliar',
      props:{
        stat: Object,
        speeds: Array,
        traits: Array,
        senses: Array,
        actions: Array,
        skills: Array,
        rulesState: Boolean,
        loadState: Boolean,
        dataState: Boolean,
      },
      components: {
      Loader,
      DeleteFam,
    },
    data() {
      return {
        actionDatas : [],
      }
    },
    methods:
    {
      checkId()
      {
        let id = this.stat.id;
        //console.log(id);
        this.$emit('delete-id', id);
      },
      updateData() {
      //console.log("Updating Data");
      //console.log(this.actions);
      this.actionDatas.length = 0;
      for (let i = 0; i < this.actions.length; i++) {
      let [type, ...details] = this.actions[i].split('/*/');
      let newDets = details[0]; 
      ////console.log(newDets);
      this.actionDatas.push(newDets);
      }
      //console.log(this.actionDatas);
    },
      statusCheck()
        {
          if (this.dataState == true)
        {
        //console.log("Data is Loaded");
         setTimeout(() => {
          this.updateData();
        }, 500);
        }
        else
        {
        //console.log("FindFam is checking again...");
        setTimeout(() => {
          this.statusCheck();
        }, 500);
        }
        },
    },
  }
</script>


<style scoped>
.goldBorder
{
   padding: 10px;
  border-width: 2px;
  border-color: #A59757;
  border-style: solid;
}
.horiz 
{
  display: inline-flex;
}

.horiz li p:after
{
  content: ",\00a0";
}

.horiz li:last-of-type p:after
{
  content: "";
}

.test
{
  height: 300px;
  width: 1000px;
}

button
{
  width: 120px;
  height: 40px;
  background-color: #A59757;
  border: none;
  border-radius: 0;
  color: #ebebebfd;
  cursor: pointer;
}
button:hover
{
  background-color: #887a3b;
  transition: all 0.3s;
}

button:active
{
  width: 120px;
  height: 40px;
}

button:visited
{
  background-color: #A59757;
  color: #ebebebfd;
}

div
{
  padding-left: 1rem;
  padding-right: 1rem;
  padding-top: 1rem;
  padding-bottom: 1rem;
  width:100%;
  display: flex;
  flex-direction: column;
  justify-content: left;
}
.gridLabel
{
margin-top: 1rem;
}
.senseGrid
{
  display: grid;
  grid-template-columns: 1fr 1fr;
  padding: 0;
  margin-top: 1rem;
}
.senseGrid p
{
  margin-left: 1rem;
  margin-right: 1rem;
  margin-bottom: 1rem;
}
.skillGrid
{
  margin-top: 1rem;
  display: grid;
  grid-template-columns: 1fr 1fr 1fr 1fr;
  padding: 0;
}
.skillGrid p
{
  margin-left: 1rem;
  margin-right: 1rem;
  margin-bottom: 1rem;
}

.bold
{
  font-weight: bold;
}

.no-padding
{
  padding-left: 0;
  padding-right: 0;
  padding-top: 0;
  padding-bottom: 0;
}

ul
{
  list-style: none;
  padding-left: 0;
}

.title-wrapper
{
  display: grid;
  grid-template-columns: 1fr 1fr;
}

.second-half
{
  display: flex;
  align-items: flex-end;
  justify-content: center;
}

.stats-wrapper ul
{
  padding-left: 0;
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  column-gap: 4rem;
  row-gap: 30px;
  margin: 0 auto;
}

.stats-wrapper ul li
{
  width: 70px;
  height: 70px;
  padding: 0.5rem;
  padding-top: calc(0.5rem - 15px) ;
  border-width: 2px;
  border-color: #A59757;
  border-style: solid;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}

.stat-mod
{
  width: 40px;
  height: 28px;
  border-width: 2px;
  padding: 0;
  border-color: #A59757;
  border-style: solid;
  border-radius: 25px;
  position: absolute;
  z-index: 2;
  display: block;
  top:55px;
  background-color: var(--color-background);
}

.stat-mod p
{
  color: #181818;
  font-weight: 500;
}

.trait-wrapper ul li
{
  padding-top: 15px;
  padding-bottom: 15px;
}

.actions-wrapper ul li
{
  padding-top: 20px;
  padding-bottom: 20px;
}

h1 {
  font-weight: 500;
  font-size: 2.6rem;
}

h3 {
  font-size: 1.2rem;
}
h2 
{
  text-align: left;
}

.wrapper
{
  --color-background: rgb(244, 242, 242);
  width: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: all, 0.2s;
}

.full-length
{
  display: flex;
  width: 100%;
}

.hideRules
{
  padding: 0;
}

.rules
{
  transition: padding-top 0.3s, padding-bottom 0.3s;
  transition-timing-function: ease-in;
}

.rules p
{
  transition: all, 0.2s;
  transition-delay: 0.3s;
  overflow-y: hidden;
}
.loadLate
{
  opacity: 0;
  height: 0;
}
.hideContent
{
  opacity: 0;
}
.loader
{
  position: absolute;
  z-index: 10;
  display: none;
  align-items: center;
  top:150px;
}
.showLoader
{
  display: flex;
}

hr
{
margin: 0.5rem;
}
.deleteBar
{
  display: flex;
  align-items: flex-end;
  justify-content: end;
}


</style>

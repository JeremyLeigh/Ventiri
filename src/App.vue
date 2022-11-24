<script setup>
//import { RouterLink, RouterView } from 'vue-router'
</script>

<template>
  <div id="logWrap" :class="{'hidden': loginState}"><LoginPage @signed-in="signedIn" /></div>
  <div id="navWrap">
    <Navigation @page-switch="pageSwitch" :pageState="pageState" />
  </div>
  <div id="homeWrap" v-if="pageState == 0">
    <div class="page homePage">
      <section id="welcomeBlock">
        <h1>Welcome {{name}}</h1>
        <br>
        <p>Conveniently predominate installed base relationships whereas timely meta-services. Credibly customize backward-compatible architectures and excellent growth strategies.<br>
        Collaboratively extend pandemic systems vis-a-vis competitive content. Quickly build orthogonal scenarios after excellent strategic theme areas. Uniquely monetize top-line communities via professional web services. Collaboratively orchestrate business sources whereas fully researched imperatives.
        </p>
      </section>
      <div class="bg" id="homeBg"><div class="bgArt"></div></div>
    </div>
  </div>
  <div id="viewWrap" v-if="pageState == 1">
    <div class="page viewPage">
      <section id="viewBlock">
        <div class="viewTop">
          <ul class="viewList topList">
          <li>
            <h3>Characters</h3>
            <ul class="viewList">
              <li><h6>Player Characters</h6></li>
              <li><h6>NPCs</h6></li>
            </ul>
          </li>
          <li>
            <h3>Creatures</h3>
            <ul class="viewList">
              <li><h6>Monsters</h6></li>
              <li><h6>Familiars</h6></li>
            </ul>
          </li>
          <li>
            <h3>Items</h3>
            <ul class="viewList">
              <li><h6>Weapons</h6></li>
              <li><h6>Magic Items</h6></li>
              <li><h6>Notes &#38; Clues</h6></li>
            </ul>
          </li>
        </ul>
        <FamiliarSelect id="famSelect" :pageState="pageState" @delete-this="deleteFam" @find-fam-id="findFamId" :allFams="allFams" />
        </div>
        <img class="hr" src="./assets/decals/hr.png">
        <div class="fogWrap"><div class="fog"></div></div>
        <div class="familiar-stats">
            <FindFamiliar ref="findFam" :dataState="dataState" @delete-id="deleteFam" :stat="stat" :speeds="speed" :senses="senses" :actions="actions" :traits="traits" :skills="skills" :rulesState="rulesState" :loadState="loadState" />
        </div>
      </section>
      <div class="bg" id="viewBg"><div class="bgArt"></div></div>
    </div>
  </div>
  <div id="editWrap" v-if="pageState == 2">
  <div class="page editPage">
    <div id="centerSection">
     <div class="modeSwitch">
      <div>
      <p @click="switchEdit" :class="{'btnActive': editTrue}">Edit</p>
      <p @click="switchCreate" :class="{'btnActive': createTrue}">Create</p>
      <div class="underline" :class="{'moveUL': createTrue}"></div>
      </div>
      <img class="hr" src="./assets/decals/hr.png">
     </div>
     <div v-if="editState == 1" class="handSection" id="editBlock">   <div class="viewTop">
          <ul class="viewList topList">
          <li>
            <h3>Characters</h3>
            <ul class="viewList">
              <li><h6>Player Characters</h6></li>
              <li><h6>NPCs</h6></li>
            </ul>
          </li>
          <li>
            <h3>Creatures</h3>
            <ul class="viewList">
              <li><h6>Monsters</h6></li>
              <li><h6>Familiars</h6></li>
            </ul>
          </li>
          <li>
            <h3>Items</h3>
            <ul class="viewList">
              <li><h6>Weapons</h6></li>
              <li><h6>Magic Items</h6></li>
              <li><h6>Notes &#38; Clues</h6></li>
            </ul>
          </li>
        </ul>
        <FamiliarSelect id="famSelectEdit" :pageState="pageState" @delete-this="deleteFam" @find-fam-id="findFamId" :allFams="allFams" />
      </div>
      <div class="familiar-stats">
     <EditFamiliar ref="editFam" :loadState="loadState" :dataState="dataState" @add-loader="this.loadState = true" @remove-loader="this.loadState = false" @postfam="addFam" :stat="stat" :speeds="speed" :senses="senses" :actions="actions" :traits="traits" :skills="skills" /></div>
      </div>
      <div v-if="editState == 2" class="handSection" id="createSection"><CreateFamiliar class="maxWidth" :dataState="dataState" ref="createfam"  @post-fam="addFam" /></div>
      </div>
    <div class="bg" id="editBg"><div class="bgArt"></div></div>
  </div>
  </div>
</template>

<script>
  import FindFamiliar from './components/FindFamiliar.vue';
  import FamiliarSelect from './components/FamiliarSelect.vue';
  import Loader from './components/Loader.vue';
  import CreateFamiliar from './components/CreateFamiliar.vue';
  import EditFamiliar from './components/EditFamiliar.vue';
  import LoginPage from './components/LoginPage.vue';
  import Navigation from './components/Navigation.vue';
  export default {
    name: 'App',
    components: {
      FindFamiliar,
      FamiliarSelect,
      Loader,
      CreateFamiliar,
      EditFamiliar,
      LoginPage,
      Navigation,
    },
    data() {
      return {
      stat: [],
      speed: [],
      senses: [],
      traits: [],
      actions: [],
      skills: [],
      allFams: [],
      rulesState: true,
      loadState: false,
      dataState: false,
      loginState: false,
      pageState: 0,
      editState: 1,
      editTrue: true,
      createTrue: false,
      confirmCode: 0,
      name: "Jeremy",
      fetchList: ['', 'speed' , 'senses', 'traits' , 'actions', 'skills'],
      }
    },
    methods:
    {
      async fetchAllFam() {
        const res = await fetch(`http://localhost:8000/api/familiar`);
        const data = await res.json();
        return data;
      },
      async fetchOneFam(id) {
        let data = [];
        for (let index = 0; index < this.fetchList.length; index++) {
          //console.log(index);
          let res = await fetch(`http://localhost:8000/api/familiar${ (this.fetchList[index] === '' ? "" : "/") + this.fetchList[index]+"/"+id}`);
          let someData = await res.json();
          data.push(someData);
          //console.log('returned data for -> ' + this.fetchList[index]);
        }
        return data;
      },
      async findFamId(id)
      {
      //console.log("Loading Starts");
      this.loadState = true;

      const allData = await this.fetchAllFam();
      const allCut = Object.values(allData);
      this.allFams = allCut;
      //console.log(this.allFams);
      //console.log(allData);

      const oneFam = await this.fetchOneFam(id);
      //console.log(oneFam);
      let eachData = [];
      for (let index = 0; index < this.fetchList.length; index++) {


        //console.log(index);
        eachData[index]  = Object.values(oneFam[index]);
        //console.log(eachData[index]);
      }
        const statConv = eachData[0];
        this.stat = statConv[0];
        this.speed = eachData[1].slice(3);
        this.senses = eachData[2].slice(4);
        this.traits = eachData[3].slice(3);
        this.actions = eachData[4].slice(3);
        this.skills = eachData[5].slice(3);
        this.dataState = true;
        if (this.pageState == 0)
        {
          this.loadState = false;
        }
        console.log("Loading Ends");
        if (this.pageState == 1)
        {
        this.loadState = false;
        this.$refs.findFam.statusCheck();
        }
          if (this.pageState == 2)
        {
        //console.log("Switch Edit Fields");
        this.$refs.editFam.statusCheck();
        }
      },
      async addFam(result) {
      //const totalFams = await this.fetchAllFam();
      result['id'] = Math.floor(Math.random() * 999999999999);
      const res = await fetch('http://localhost:8000/api/familiar', {
        method: 'POST',
        headers: {
          'Content-type': 'application/json',
          'Accept': 'application/json',
        },
        body: JSON.stringify(result)
      }).then((response) => { this.confirmCode = response.status});
      const data = await result;
      //console.log(data);
      this.allFams = [...this.allFams, data];
      this.$refs.createfam.uploadStatus(this.confirmCode);
    },
      async deleteFam(id) {
        //console.log(id);
        fetch(`http://localhost:8000/api/familiar/${id}`, {
          method: 'DELETE'
        })
        .then(res => res.text()) // or res.json()
        .then(res => console.log(res)
        
        );
      const res = await this.fetchAllFam();
      let firstId = res[0].id;
      this.findFamId(firstId);
    },
    signedIn()
    {
      //console.log("signed in");
      this.loginState = true;
      //console.log(this.loginState);
    },
       pageSwitch(pNum)
    {
      console.log("Page Switched to "+pNum);
      this.pageState = pNum;
    },
      toTop()
      {
        //console.log("Window was Resized");
        document.body.scrollTop = 0; // For Safari
        document.documentElement.scrollTop = 0; // For Chrome, Firefox, IE and Opera
      },
      switchEdit()
      {
        this.editState = 1;
        this.editTrue = true;
        this.createTrue = false;
      },
      switchCreate()
      {
        this.editState = 2;
        this.editTrue = false;
        this.createTrue = true;
      },
    },
    //LIFECYCLE METHODS
    async created() {
      const res = await this.fetchAllFam();
      let id = res[0].id;
      this.findFamId(id);
      window.addEventListener("resize", this.toTop); 
    },
  }
</script>



<style>


@import '@/assets/base.css';

/* width */
::-webkit-scrollbar {
  width: 8px;
  height: 8px;
}

/* Track */
::-webkit-scrollbar-track {
  background: #e9e6e6;
  border-radius: 15px;
}

/* Handle */
::-webkit-scrollbar-thumb {
  background: rgb(190, 190, 190);
  border-radius: 15px;
}

/* Handle on hover */
::-webkit-scrollbar-thumb:hover {
  background: rgb(156, 155, 155);
}


.maxWidth
{
  width:100%;
}

/*View Page*/

#viewBlock #famSelect
{
  margin-top:60px;
}

.fog
{
  width:100%;
  height: 60px;
  position: absolute;
  z-index: 200;
  background: rgb(255,255,255,0);
  background: linear-gradient(180deg, rgba(255,255,255,1) 30%, rgba(255,255,255,0) 100%);
}

.fogWrap
{
  width:100%;
  margin-left: 48px;
  margin-right: 48px;
  height: 0;
  position: relative;
}

.hr
{
  display: flex;
  justify-self: center;
  align-self: center;
  width: 200%;
  max-width: 1200px;
  opacity: 0.9;
  padding-left: 48px;
  padding-right: 48px;
}

.topList
{
  margin-top:60px;
  padding-left: 48px;
  margin-right:30px;
}

#famSelect
{
  width: 100%;
  margin:0;
  margin-right:48px;
}

#famSelectEdit
{
  width: 100%;
  margin:0;
  
}

#viewBlock
{
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  height: 100%;
  width:100vw;
  margin-top: 150px;
}

.viewTop
{
  margin-top: 10px;
  margin-right:20px;
  display: flex;
  flex-direction: row;
  width:100%;
}

.viewList
{
  display: flex;
  justify-content: flex-start;
  align-items: flex-start;
  flex-direction: column;
  list-style-type:none;
}

.viewPage
{
  justify-content:flex-end;
  flex-direction: row-reverse;
}

h6
{
  cursor: pointer;
  transition: all 0.1s;
}

h6:hover
{
  color: #A49758;
  transition: all 0.1s;
}

h6::before
{
  content:'';
  background-image: url("/assets/icons/bullet.png");
  width:17px;
  height: 17px;
  position: absolute;
  left:-20px;
  top:15%;
  z-index: 300;
  background-size: contain;
  transform: rotateX(90deg);
  transition: all 0.1s;
}

h6:hover::before
{
transform: rotateX(0deg);
transition: all 0.2s;
}

/*Edit Page */

.underline
{
  background-color: #A49758;
  height: 2px;
  width: 45px;
  position: absolute;
  bottom: 15px;
  left: -3px;
  transition: all 0.3s;
}

.moveUL
{
  left:58px;
  transition: all 0.3s;
}

.modeSwitch
{
  width: 100%;
  height:130px;
  padding-top:30px;
  padding-left:48px;
  padding-right:48px;
  display: flex;
  flex-direction: column;
  overflow: hidden;
}

.modeSwitch div
{
  display: flex;
  flex-direction: row;
}

.modeSwitch p
{
  margin-right: 15px;
  background-color: transparent;
  cursor: pointer;
  color: #887A3B;
  padding:5px;
  transition: all 0.3s;
}

.btnActive
{
color: #A59757;

}

.modeSwitch img
{
  display: flex;
  justify-self: center;
  align-self: center;
}

#editBlock
{
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  height: 80%;
  width:100vw;
  padding:20px;
  padding-top:0px;
}

#createSection
{
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  height: 80%;
  width:100vw;
  padding:20px;
  padding-top:60px;
}

#centerSection
{
  display: flex;
  align-items: center;
  flex-direction: column;
  height: 100%;
  width:100vw;
  margin-top:200px;
}

.editPage
{
  justify-content:flex-end;
  flex-direction: row-reverse;
}

#editBg
{
  width: 0vw;
  height: 100%;
  transition: all 0.3s;
}

#editBg div
{
  background-image: url("/assets/bgs/market.jpg");
  background-position: center;
  background-size: cover;
  -webkit-mask-image: url("/assets/stains/stain3.png");
  mask-image: url("/assets/stains/stain2.png");
   -webkit-mask-size: cover;
  mask-size: cover;
  -webkit-mask-position: bottom;
  mask-position: bottom;
  -webkit-mask-repeat: no-repeat;
  mask-repeat: no-repeat;
}

/*All Pages*/
#homeBg
{
  width: 100vw;
  height: 50vh;
}

.bg div
{
  background-repeat: no-repeat;
  width: 100%;
  height: 100%;
}

#homeBg div
{
  background-image: url("/assets/bgs/farm.jpg");
  background-position: center;
  background-size: cover;
  -webkit-mask-image: url("/assets/stains/stain2.png");
  mask-image: url("/assets/stains/stain2.png");
   -webkit-mask-size: cover;
  mask-size: cover;
  -webkit-mask-position: top;
  mask-position: top;
  -webkit-mask-repeat: no-repeat;
  mask-repeat: no-repeat;
}

#viewBg
{
  width: 0vw;
  height: 100%;
  transition: all 0.3s;
}

#viewBg div
{
  background-image: url("/assets/bgs/castle.jpg");
  background-position: center;
  background-size: cover;
  -webkit-mask-image: url("/assets/stains/stain3.png");
  mask-image: url("/assets/stains/stain2.png");
   -webkit-mask-size: cover;
  mask-size: cover;
  -webkit-mask-position: bottom;
  mask-position: bottom;
  -webkit-mask-repeat: no-repeat;
  mask-repeat: no-repeat;
}

body
{
  margin: 0 auto;
  width: 100vw;
}

.hidden
{
  animation-name: leaveUp;
  animation-duration: 1s;
  z-index: 105;
  animation-fill-mode: forwards;
  position: absolute;
}

@keyframes leaveUp {
  0%   {transform: translateY(0);}
  100% {transform: translateY(-100vh);}
}

.page
{
  display: flex;
  align-items: center;
  width: 100vw;
  background-color: #ffffff;
  color: #333333;
}

.homePage
{
   justify-content:flex-end;
  flex-direction: column;
}

.page h1
{
  color:#A59757;
  font-size: 2.8rem;
}

#welcomeBlock
{
  max-width: 600px;
  width: 80vw;
  text-align: center;
  position: relative;
  
}

#navWrap
{
  position: absolute;
  top: 0;
  left: 0;
  z-index: 99;
}

.familiar-stats
{
  width:100%;
}

/*@media (min-width: 1024px) {
  #app {
    display: grid;
    grid-template-columns: 1fr 1fr;
  }
}*/

button, .submit
{
  width: 100px;
  height: 40px;
  background-color: #A59757;
  border: none;
  color: #ebebebfd;
  cursor: pointer;
  transition: all 0.3s;
}

button:active, .submit:active
{
  width: 100px;
  height: 40px;
  background-color: #A59757;
  color: #ebebebfd;
  cursor: pointer;
  transition: all 0.3s;
}

button:hover, .submit:hover
{
  background-color: #887a3b;
  transition: all 0.3s;
}

.submit
{
  margin-left: 10px;
}

h2
{
  text-align: center;
}

/*@media (prefers-color-scheme: dark) {
  body, div {
    background-color: #1d1d1d;
    color: #ebebebb0;
  }

  input, text, select, option, textarea {
    background-color: #ebebeb;
  }
}*/

@media  screen and (min-width: 950px) {
  .hr
{
  width: 100%;
  max-width: 1200px;
  padding-left: 48px;
  padding-right: 48px;
}
}

@media  screen and (min-width: 675px) {
#viewBg
{
  width: 30vw;
  height: 100%;
  transition: all 0.3s;
}
#editBg
{
  width: 30vw;
  height: 100%;
  transition: all 0.3s;
}

#viewBlock
{
  display: flex;
  justify-content: flex-start;
  align-items: flex-start;
  flex-direction: column;
  height: 100%;
  overflow-y: scroll;
  overflow-x: hidden;
  width:78vw;
  margin-top: 0px;
}

#editBlock
{
  display: flex;
  justify-content: flex-start;
  align-items: flex-start;
  flex-direction: column;
  height: 85%;
  overflow-y: scroll;
  overflow-x: hidden;
  width:78vw;
  margin-top: 0px;
}

#createSection
{
  display: flex;
  justify-content: flex-start;
  align-items: flex-start;
  flex-direction: column;
  overflow-y: scroll;
  overflow-x: hidden;
  height: 85%;
  width:78vw;
  margin-top: 0px;
}


.familiar-stats
{
  width:100%;
  margin: 0 auto;
  height:100%;
}

.page
{
  height: 100vh;
}

#centerSection
{
  margin-top:0px;
}

}
</style>

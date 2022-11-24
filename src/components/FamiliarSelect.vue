<script setup>
//import { RouterLink, RouterView } from 'vue-router'
</script>

<template>
    <div>
        <DeleteFam :class="{'hidden': (pageState == 1)}" @toggle-delete="toggleDel" />
        <ul>
            <li :style="{backgroundImage: 'url('+'./assets/fam_art/'+allFam['creature-name']+'.jpg'+')'}" v-on:click="sendId(allFam.id)" :key="allFam.id" v-for="allFam in allFams">
            <p>{{allFam["unique-name"] != null ? allFam["unique-name"] : allFam["creature-name"]}}</p>
            <span :class="{ 'showDelete': deleteState}" :key="allFam.id" v-on:click="deleteThis(allFam.id)">&#10005;</span>
            </li>
        </ul>
    </div>
</template>

<script>
  import DeleteFam from './DeleteFam.vue';
  export default {
      name: 'FamiliarSelect',
      props:{
          allFams: Array,
          pageState: Number,
      },
      methods: {
          toggleDel() {
              this.deleteState = true;
          },
          deleteThis(id)
          {
            //console.log(id);
            this.$emit("delete-this", id);
            //console.log("Sending Id to be deleted");
          },
          sendId(id)
          {
            if (!this.deleteState)
            this.$emit("find-fam-id", id);
          },
      },
        components: {
        DeleteFam
    },
    data() {
      return {
        deleteState: false,
    }
    }
  }
</script>

<style scoped>

.hidden
{
    display: none;
}

li {
    list-style: none;
    display: inline-flex;
    width: 100px;
    height: 100px;
    padding: 0.5rem;
    margin-left: 20px;
    margin-right: 20px;
    margin-bottom: 20px;
    border-width: 2px;
    border-color: #A59757;
    border-style: solid;
    justify-content: center;
    align-items: center;
    text-align: center;
    transition: transform 0.3s;
    background-size: contain;
    background-position: center;
    filter: brightness(90%);
}

li:hover {
    transform:scale(1.2);
    transition: transform 0.3s;
    cursor: pointer;
}

p
{
 background-color: #00000090;
 padding-left: 5px;
 padding-right: 5px;
 color: azure;
 margin-bottom: 0;
}

ul {

    padding: 30px;
    border-width: 2px;
    border-color: #A59757;
    border-style: solid;
    width:100%;
    height:80%;
    overflow-y:scroll;
}
span {
    color: #A59757;
    position: absolute;
    top:-5px;
    right:0px;
    padding: 5px;
    z-index: 3;
    display: none;
    cursor: pointer;
    font-weight: bold;
}

.showDelete
{
    display: block;
    pointer-events: all;
}

div {
    padding: 0;
}
</style>
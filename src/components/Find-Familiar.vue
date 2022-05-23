<template>
  <div class="wrapper">
      <div class="creature-wrapper">
        <div class="title-wrapper">
          <h2>Name</h2>
          <h4>Creature Type</h4>
          <p>Creature Size/Category, Alignment</p>
        </div>
        <hr>
        <div class="first-wrapper">
          <p>Armor Class: Armor Class</p>
          <p>Hit Points: Number (Number d6)</p>
          <p>Speed: speed1, speed2, ...</p>
        </div>
        <hr>
        <div class="stats-wrapper">
          <ul>
            <li>STR #</li>
            <li>DEX #</li>
            <li>CON #</li>
            <li>INT #</li>
            <li>WIS #</li>
            <li>CHA #</li>
          </ul>
        </div>
        <hr>
        <div class="third-wrapper">
          <p>Skills: Skill1 #, Skill2 #</p>
          <p>Senses: Sense1 #, Sense2 #</p>
          <p>Passive Perception: #</p>
        </div>
        <hr>
        <div class="trait-wrapper">
          <ul>
            <li><h4>Trait1</h4><p>desc</p></li>
            <li><h4>Trait2</h4><p>Trait1: desc</p></li>
            <li><h4>Trait3</h4><p>Trait1: desc</p></li>
          </ul>
        </div>
        <hr>
        <div class="actions-wrapper">
          <h3>Actions</h3>
          <ul>
            <li><h4>Action1</h4><p></p></li>
            <li><h4>Action1</h4><p></p></li>
            <li><h4>Action1</h4><p></p></li>
          </ul>
        </div>
        <div>Basic Rules Section w/ Reveal Button</div>
      </div>
    </div>
</template>

<script setup>
defineProps({

})
</script>


<style scoped>

div
{
  padding-left: 1rem;
  padding-right: 1rem;
  padding-top: 1.5rem;
  padding-bottom: 1.5rem;
  width:100%;
  display: flex;
  flex-direction: column;
  justify-content: left;
}

ul
{
  list-style: none;
  padding-left: 0;
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
  border-width: 2px;
  border-color: brown;
  border-style: solid;
  display: flex;
  justify-content: center;
  align-items: center;
}

h1 {
  font-weight: 500;
  font-size: 2.6rem;
}

h3 {
  font-size: 1.2rem;
}

.wrapper
{
  background-color: rgb(244, 242, 242);
  width: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
}

</style>

<template>
    <section id="navBlock" @mouseover="openNav" @mouseleave="closeNav">
        <div ref="navClick" id="navClickBox">
            <div id="imgWrap">
            <img v-if="pageState == 0" class="imgV vMod" src="../assets/logo/v.png">
            <img v-if="pageState == 1" class="imgV eyeMod" src="../assets/logo/eye.png">
            <img v-if="pageState == 2" class="imgV handMod" src="../assets/logo/hand.png">
            <img v-if="pageState == 3" class="imgV hatMod" src="../assets/logo/hat.png">
            <div ref="navBg" id="navBg" class="navClose">
            </div>
            </div>
        </div>
        <div ref="navSpinner" id="navSpinner">
                <a @click="switchView" href="#" id="eyeWrap" class="magnetic-wrap">
                    <div ref="eyeArea" class="magnetic-area magnetic-size magEye"></div>
                    <div class="magnetic-content1 magnetic-content">
                    <p id="eyeText" class="ml13 text1">View</p>
                    <img id="navEye" src="../assets/logo/eye.png">
                    </div>
                </a>
                <a @click="switchEdit" href="#" id="handWrap" class="magnetic-wrap">
                    <div ref="handArea" class="magnetic-area magnetic-size magHand"></div>
                    <div class="magnetic-content2 magnetic-content">
                    <p id="handText" class="ml13 text2">Edit</p>
                    <img id="navHand" src="../assets/logo/hand.png">
                    </div>
                </a>
                <a @click="switchPlay" href="#" id="hatWrap" class="magnetic-wrap">
                <div ref="hatArea" class="magnetic-area magnetic-size magHat"></div>
                <div class="magnetic-content3 magnetic-content">
                <p id="hatText" class="ml13 text3">Play</p>
                <img id="navHat" src="../assets/logo/hat.png">
                </div>
                </a>
        </div>
    </section>
</template>

<script>
export default {
      name: 'Navigation',
      props:{
        pageState: Number,
      },
      components: {
    },
    data() {
      return {
        navState: false,
      }
    },
    mounted() {
//Magnetic Buttons
var mArea = [this.$refs['eyeArea'], this.$refs['handArea'], this.$refs['hatArea']];
//console.log(mArea[0]);
// 1. Set the function and variables
function parallaxIt(e, target, movement = 1){
  let newTarget = '.magnetic-content'+target.toString();
  //console.log(newTarget);
  var boundingRect = e.srcElement.getBoundingClientRect();
  var relX = e.pageX - boundingRect.left;
  var relY = e.pageY - boundingRect.top;
  var scrollTop = window.pageYOffset || document.documentElement.scrollTop;

  gsap.to(newTarget, {
    x: (relX - boundingRect.width/2) * movement,
    y: (relY - boundingRect.height/2 - scrollTop) * movement,
    ease: "power1",
    duration: 0.6
  });
}

// 2. Call the function
function callParallax(e, target){
  parallaxIt(e, target);
}

for (let i = 0; i < mArea.length; i++) {
mArea[i].addEventListener('mouseenter', function(){
  let target = i+1;
  textReveal(target);
});   
}

for (let i = 0; i < mArea.length; i++) {
mArea[i].addEventListener('mousemove', function(e){
  let target = i+1;
  callParallax(e, target);
});   
}

for (let i = 0; i < mArea.length; i++) {
mArea[i].addEventListener('mouseleave', function(e){
  gsap.to('.magnetic-content', {
    scale:1,
    x: 0,
    y: 0,
    ease: "power3",
    duration: 0.6
  });
  let target = i+1;
  textHide(target);
  //console.log("Mouse Left from "+(i+1));
});
}

// Wrap every letter in a span
var textWrapper = document.querySelectorAll('.ml13');
//console.log(textWrapper[0]);
for (let i = 0; i < textWrapper.length; i++) {
    textWrapper[i].innerHTML = textWrapper[i].textContent.replace(/\S/g, "<span class='letter'>$&</span>");
}

var textLetter = document.querySelectorAll('.letter');
//console.log(textLetter);
for (let i = 0; i < textLetter.length; i++) {
    textLetter[i].style.opacity = "0";
}



function textReveal(target) {
let newTarget = '.text'+target.toString()+' .letter';
anime({
    targets: newTarget,
    //translateY: [100,0],
    //translateZ: 0,
    opacity: [0,1],
    easing: "easeOutExpo",
    duration: 600,
    delay: (el, i) => 20 + 30 * i
  })
  //console.log("showing text");
  }
  
  function textHide(target) {
    let newTarget = '.text'+target.toString()+' .letter';
    anime({
    targets: newTarget,
    //translateY: [0,-100],
    opacity: [1,0],
    easing: "easeInExpo",
    duration: 600,
    delay: (el, i) => 20 + 30 * i
  })
  for (let i = 0; i < textLetter.length; i++) {
    textLetter[i].style.opacity = "0";
}
  //console.log("hiding text");
  }

    },
    methods: {
        
        openNav()
        {
            //console.log("opening nav");
            this.$refs.navBg.classList.remove("navClose");
            this.$refs.navBg.classList.add("navOpen");
            this.$refs.navSpinner.classList.remove("spinOut");
            this.$refs.navSpinner.classList.add("spinIn");
        },
        closeNav()
        {
            //console.log("closing nav");
            this.$refs.navSpinner.classList.remove("spinIn");
            this.$refs.navSpinner.classList.add("spinOut");
            setTimeout(() => {
            this.$refs.navBg.classList.remove("navOpen");
            this.$refs.navBg.classList.add("navClose");
            }, 200);
        },
        switchView()
        {
            let pNum = 1;
            this.$emit('page-switch', pNum);
        },
         switchEdit()
        {
            let pNum = 2;
            this.$emit('page-switch', pNum);
        },
         switchPlay()
        {
            let pNum = 3;
            this.$emit('page-switch', pNum);
        },
    }
}

</script>

<style scoped>

#navBlock
{
    display: flex;
    justify-content: center;
    align-items: center;
    width: 100vw;
    height: 170px;
}

#navClickBox
{
 position: absolute;
 cursor: pointer;
 border-radius: 50%;
 height: 141px;
 padding: 30px;
 width:100vw;
 display: flex;
 justify-content: center;
 justify-content: flex-start;
}

.imgV
{
    position: relative;
    z-index: 150;
    transform: scale(0.8);
}

#imgWrap
{
    height: 100px;
    width: 100px;
    display: flex;
    justify-content: center;
    align-items: center;
}

#navBg
{
    position: absolute;
    z-index: -1;
    background-color: #1d1d1d;
    transition: all 0.6s;
    top:-20px;
    left:-15px;
    border-top-left-radius : 80px;
    border-top-right-radius : 80px;
    border-bottom-left-radius : 80px;
    border-bottom-right-radius : 80px;
    height:130px;
    width: 130px;
}

#navSpinner
{
    width: 60vw;
    height: 100px;
    border-radius: 200px;
    position: absolute;
    z-index: 100;
    display: flex;
    justify-content:space-between;
    align-items: center;
    pointer-events: none;
    flex-direction: row;
    transform: translateX(-150vw);
}

.spinOut
{
    animation-name: spinOut;
    animation-fill-mode: forwards;
    animation-duration: 0.3s;
}

.spinIn
{
    animation-name: spinIn;
    animation-fill-mode: forwards;
    animation-duration: 0.3s;
    animation-delay: 0.2s;
}

#navSpinner a
{
    position: relative;
    height: 100px;
    width: 100px;

}

#navSpinner img
{
transform: scale(0.5);
}


.navOpen
{
    animation-name: openNav;
    animation-fill-mode: forwards;
    animation-duration: 0.3s;
}

.navClose
{
    animation-name: closeNav;
    animation-fill-mode: forwards;
    animation-duration: 0.4s;
}

@keyframes openNav {
    from 
    {
    width: 130px;
    height: 130px;
    }
    to 
    {
    width: 90vw;
    }
}

@keyframes closeNav {
    from 
    {
    width: 90vw;
    }
    to 
    {
    width: 130px;
    height:130px;
    }
}





/*Magnetic Buttons*/

.magnetic-content
{
    position: absolute;
    top:0;

}

.magnetic-wrap {
  display: block;
  position: relative;
  justify-content: center;
  align-items: center;
}

.magnetic-content p {
    font-weight: normal;
    color: #A49758;
    text-decoration: none;
    text-align: center;
    position: absolute;
    z-index: 90;
}

#eyeText {
    top:5px;
    left:33px;
}

#handText {
 top:-5px;
 left:35px;
}

#hatText {
 top:3px;
 left:35px;
}

.magnetic-size {
  position: relative;
  width: 80px;
  height: 80px;
  z-index: 300;
  border-radius: 50%;
  pointer-events: all;
}

@keyframes spinIn {
    from 
    {
    transform: translateX(-150vw);
    }
    to 
    {
    transform: translateX(5vw);
    }
}

@keyframes spinOut {
    from 
    {
    transform: translateX(5vw);
    }
    to 
    {
    transform: translateX(-150vw);
    }
}

#navEye
{
 right:3px;
 top:10px
}

#navHand
{
 left:17px;
 bottom:10px;
}

#navHat
{
 right:17px;
 top:10px;
}

.ml13 .letter {
  display: inline-block;
  line-height: 1em;
}

@media  screen and (min-width: 675px) {
@keyframes openNav {
    from 
    {
    width: 130px;
    height: 130px;
    }
    to 
    {
    width: 400px;
    height: 400px;
    }
}

@keyframes closeNav {
    from 
    {
    width: 400px;
    height: 400px;
    }
    to 
    {
    width: 130px;
    height:130px;
    }
}

#navBg
{
    position: absolute;
    z-index: -1;
    background-color: #1d1d1d;
    transition: all 0.6s;
    left: 50%;
    top: 50%;
    transform: translate(-50%, -50%);
    border-top-left-radius : 50%;
    border-top-right-radius : 50%;
    border-bottom-left-radius : 50%;
    border-bottom-right-radius : 50%;
    height:130px;
    width: 130px;
}

#navSpinner
{
    width: 400px;
    height: 400px;
    border-radius: 200px;
    position: absolute;
    z-index: 100;
    display: flex;
    justify-content: flex-end;
    align-items: flex-end;
    pointer-events: none;
    flex-direction: column;
    transform: rotate(-180deg);
}
#navEye
{
 top:0px;
 right:13px;
}

#navHand
{
 bottom:20px;
 left:8px;
}

#navHat
{
 top:0px;
 right:25px;
}

#eyeWrap
{
 top:30px;
 right:10px;
}

#handWrap
{
 right:15px;
 top:20px;
}

#hatWrap
{
 top: -10px;
 right: 90px;
}

@keyframes spinIn {
    from 
    {
    transform: rotate(-100deg);
    }
    to 
    {
    transform: rotate(0deg);
    }
}

@keyframes spinOut {
    from 
    {
    transform: rotate(0deg);
    }
    to 
    {
    transform: rotate(180deg);
    }
}
#navClickBox
{
 position: absolute;
 cursor: pointer;
 border-radius: 50%;
 padding: 30px;
 width:100%;
 justify-content: center;
}
#navBlock
{
    display: flex;
    justify-content: center;
    align-items: center;
    width: 170px;
    height: 170px;
}

#eyeText {
    top:-5px;
    left:22px;
}

#handText {
 top:-10px;
 left:25px;
}

#hatText {
 top:-5px;
 left:25px;
}

}

</style>
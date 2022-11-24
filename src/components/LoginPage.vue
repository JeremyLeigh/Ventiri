<template>
    <div id="splash">
        <p id="verCode">v0.0.12 3000</p>
        <section id="logo">
            <img id="plus" src="../assets/logo/plus.png">
            <div>
            <img src="../assets/logo/hand.png">
            <img src="../assets/logo/hat.png">
            <img src="../assets/logo/v.png">
            <img src="../assets/logo/eye.png">
            </div>
        </section>
        <section id="login">
            <button ref="loginBtn" @click="openLogin" id="loginBtn">LOGIN</button>
            <div ref="container" id="container">
            <form action="action_page.php" method="post">
                <div class="formRow">
                   <input type="text" name="text" autocomplete="off" required />
                    <label for="text" class="label-name">
                        <span class="content-name">Username</span>
                    </label>
                </div>
                <div class="formRow">
                    <input type="text" name="text" autocomplete="off" required />
                    <label for="text" class="label-name">
                        <span class="content-name">Password</span>
                    </label>
                </div>
            </form>
            <button @click="signedIn" type="submit">ENTER</button>
            </div>
        </section>
    </div>
</template>

<script>
import Loader from './Loader.vue';
export default {
      name: 'LoginPage',
      props:{
        dataState: Boolean,
      },
      components: {
      Loader,
    },
    data() {
      return {
        dropState: false,
        loginStatus: false,
      }
    },
    methods: {
        openLogin()
        {
            if (this.dropState == false) {
            this.$refs.container.style.height = 185+"px";
            this.$refs.loginBtn.innerHTML = "EXIT";
            this.dropState = true;
            }
            else
            {
            this.$refs.container.style.height = 0+"px";
            this.$refs.loginBtn.innerHTML = "LOGIN";
            this.dropState = false;
            }
            
        },
        signedIn()
        {
          this.$emit('signed-in');
        }
    }
}
</script>

<style scoped>
#splash
{
    background-color: white;
    background-image: url("../assets/bgs/marble.jpg");
    background-size: cover;
    width: 100vw;
    height: 100vh;
    position: absolute;
    z-index: 100;
    display: flex;
    align-items: center;
    justify-content: center;
    flex-direction: column;
}

#verCode
{
    color: #ada684;
    position: absolute;
    right: 50px;
    top: 15px;
}

#logo
{
    background: none;
    max-width: 400px;
    max-height: 400px;
    width: 80%;
    height: 80%;
    position: relative;
   
}
#logo div
{
    background-color: inherit;
    display: grid;
    grid-template-columns: 1fr 1fr;
    align-items: center;
    padding: 30px;
    width: 100%;
    height: 100%;
    grid-template-rows: 50% 50%;
    background-size: contain;
}

@media only screen and (min-width: 500px) {
  #logo div {
    background-image: url('../assets/logo/border.png');
  }
}

#logo div img
{
    margin: 0 auto;
}

#plus
{
    position: absolute;
    z-index: 101;
    left:calc(50% - 28px);
    top:calc(50% - 28px);
}

#login
{
    background:none;
    display: flex;
    flex-direction: column;
    align-items: center;
}

#container
{
    background:none;
    margin-top: 20px;
    display: flex;
    flex-direction: column;
    align-items: center;
    height: 0;
    transition: all 0.3s;
    overflow: hidden;
}


.formRow
{
    margin-bottom: 15px;
    overflow-x: hidden;
    background:none;
}

.formRow:last-of-type
{
    margin-bottom: 25px;
}

form input {
  width: 300px;
  height: 50px;
  border: none;
  background:none;
  padding-top:20px;
  color: #1d1d1d;
}

form label {
  position: absolute;
  bottom: 0px;
  left: 0px;
  width: 100%;
  height: 100%;
  pointer-events: none;
  border-bottom: 1px solid rgb(184, 175, 175);
  color: rgb(138, 135, 135);

}
form label::after {
  content: "";
  position: absolute;
  bottom: -1px;
  left: 0px;
  width: 100%;
  height: 100%;
  border-bottom: 3px solid #A59757;
  transform: translateX(-100%);
  transition: all 0.3s ease;
}

.content-name {
  position: absolute;
  bottom: 0px;
  left: 0px;
  padding-bottom: 5px;
  transition: all 0.3s ease;
}

form input:focus {
  outline: none;
}

form input:focus + .label-name .content-name,
form input:valid + .label-name .content-name {
  transform: translateY(-20px);
  font-size: 14px;
  left: 0px;
  color: #A59757;
}

form input:focus + .label-name::after,
form input:valid + .label-name::after {
  transform: translateX(0%);
}

button
{
    color: white;
    font-weight: 400;
}

#loginBtn
{
    background: none;
    color:#A59757;
    font-weight:600;
    font-size: 16px;
    margin-top: 20px;
}

</style>

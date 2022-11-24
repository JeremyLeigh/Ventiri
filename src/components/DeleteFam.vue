
<template>
    <div class="deleteWrap">
        <button @click="deleteThis">Delete</button>
    </div>
</template>

<script>
  export default {
      name: 'DeleteFam',
      props:{
         // stat: Object,
      },
         data(){
        return {
        
        }
      },
      methods: {
            deleteThis() {
                if(confirm("Are you sure you want to delete a Familiar"))
                {
                console.log("Sending id and toggling");
                this.$emit('delete-id');
                this.$emit('toggle-delete');
                }
          }
      },
  }
</script>

<style scoped>
p
{
 background-color: #00000090;
 padding-left: 5px;
 padding-right: 5px;
 color: azure;
 margin-bottom: 0;
}
.deleteWrap
{
    display: flex;
    align-items: center;
    justify-content: end;
}
.deleteWrap button
{
    margin-top: 20px;
}

</style>
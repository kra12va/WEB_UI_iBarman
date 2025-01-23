

<template>
    <form class="form" @submit.prevent>
      <div class="form">
        <my-input 
          type="text" 
          v-model="post.title" 
          placeholder="Название"
        />
        <my-input 
          type="text" 
          v-model="post.body" 
          placeholder="Описание"
        />
        <my-button
          style="align-self: flex-end"
          @click="creatPost"
        >Сохранить</my-button>
      </div>
    </form>
    
</template>
  
<script>
import MyButton from './UI/myButton.vue';


export default {

    data() {
        return {
            post: {
                title: '',
                body: ''
            }
        }
    },
    methods: {
        creatPost(){
            this.post.id = Date.now()
            this.$emit('create', this.post)
            this.post = {
                title: '',
                body: ''    
            }            
        }
    }
}
  
</script>
  
  
<style scoped>


.form{
  display: flex;
  flex-direction: column;

}


</style>
  
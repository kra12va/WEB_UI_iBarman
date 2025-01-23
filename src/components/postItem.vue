

<template>
    <div class="post">
        <div>
            <div class="postItem"><strong>Название: {{ post.title }}</strong></div>
            <div class="postItem"><strong>Описание: {{ post.body }}</strong></div>
        </div>
        <div class="post_btn">
           <my-button @click="$emit('remove',post)">Удалить</my-button>
        </div>
    </div>
</template>
  
<script>


export default {

    props: {
        post: {
            type: Object,
            required: true
        }
    }

}
  
</script>

<style scoped>
.post {
  height: 100%; /* Высота */ 
  background: #342929;
  margin: 5px;
  padding: 15px;
  border: 3px solid #027428;
  display: flex;
  align-items: center;
  justify-content: space-between;
}
</style>

  
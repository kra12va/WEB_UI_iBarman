<template>
    <div v-if="posts.length > 0">
        <h3>Список пользователей:</h3>
        <postItem
            v-for="post in posts"
            :post="post"
            :key="post.id"
            @remove="$emit('remove',post)"
        />

    </div>
    <h3 v-else style="color: red">Список пуст</h3>
</template>

<script>
import postItem from './postItem.vue';
export default {
    components:{postItem},
    props:{
        posts: {
            type: Array,
            required: true
        }
    }
}

</script>


<style scoped>

</style>  
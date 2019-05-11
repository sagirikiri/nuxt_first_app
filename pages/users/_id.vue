<template>
    <section>
        <div>
            <h3>{{ user.id }}</h3>
            <img :src="user.profile_image_url" >
            <p>{{ user.description || '説明文なし' }}</p>
            <p>
                <nuxt-link to="/">
                    <small><b>トップに戻る</b></small>
                </nuxt-link>
            </p>
            <h3>{{ user.id }}</h3>
            <ul>
                <li v-for="item in items"
                v-bind:key="item.id">
                    <h4>
                        <span>{{ item.title }}</span>
                    </h4>
                    <div>
                        {{item.body.slice(0,130)}}
                    </div>
                    <p>
                        <a target="_blank" :href="item.url">{{ item.url }}</a>
                    </p>
                </li>
            </ul>
        </div>
    </section>
</template>

<script>
    import { mapGetters } from 'vuex'
export default {
   async asyncData({route,store,redirect}){
       if(store.getters['users'][route.params.id]){
           return;
       }
       try{
           await store.dispatch('fetchUserInfo', {id: route.params.id})
       } catch(e){
           redirect('/');
       }
   },
   computed: {
       user() {
           return this.users[this.$route.params.id]
       },
       items() {
           return this.userItems[this.$route.params.id]||[]
       },
       ...mapGetters(['users','userItems'])
   }
}
</script>

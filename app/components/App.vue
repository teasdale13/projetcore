<template>
    <Page>
        <ActionBar >
          <label class="actionbarTitle" text="NetFilm"/>
        </ActionBar>
        <StackLayout>
          <Label class="message" text="Je suis la page d'accueil"/>
          <button text="Liste de Films" @tap="onButtonListTap"/>
          <button text="Liste des films prêtés" @tap="onSharedListTap"/>
          <button text="Liste de amis" @tap="onFriendListTap"/>
        </StackLayout>
            
    </Page>
</template>

<script >
import movieslistpage from "./MoviesListPage";
import friendlist from "./FriendList";
import sharedmovies from "./SharedMovies";

  export default {
    data() {
      return {
        msg: ""
      }
    },
    methods: {
    onButtonListTap: function (event){
      this.$navigateTo(movieslistpage);
    },
    onFriendListTap: function(event){
      this.$navigateTo(friendlist);
    },
        onSharedListTap: function(event){
      this.$navigateTo(sharedmovies)
    }
  }
  }
  

</script>

<style scoped>

    .message {
        vertical-align: center;
        text-align: center;
        font-size: 20px;
        color: #333333;
    }
</style>

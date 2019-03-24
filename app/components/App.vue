<template>
    <Page @loaded="pageLoaded">
        <ActionBar >
          <label class="actionbarTitle" text="NetFilm"/>
        </ActionBar>
        <StackLayout>
          <Label class="message" text="Je suis la page d'accueil"/>
          <button text="Liste de Films" @tap="onButtonListTap" class="button"/>
          <button text="Liste des films prêtés" @tap="onSharedListTap" class="button"/>
          <button text="Liste de amis" @tap="onFriendListTap" class="button"/>
          <button text="Extra" @tap="onExtraTap" class="button"/>
        </StackLayout>
            
    </Page>
</template>

<script >
import movieslistpage from "./MoviesListPage";
import friendlist from "./FriendList";
import sharedmovies from "./SharedMovies";
import extra from "./Form";
import * as app from 'tns-core-modules/application'
import * as platform from 'tns-core-modules/platform'
import * as color from 'tns-core-modules/color'

  export default {
    data() {
      return {
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
    },

    onExtraTap: function(){
        this.$navigateTo(extra)
    },

    pageLoaded() {
        if (app.android && platform.device.sdkVersion >= "21") {
            const window = app.android.foregroundActivity.getWindow();
            window.setStatusBarColor(new color.Color("#000000").android);
        }
    },
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

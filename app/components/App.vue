<template>
    <Page>
        <ActionBar >
          <label class="actionbarTitle" text="NetFilm"/>
        </ActionBar>
        <StackLayout>
          <Label class="message" text="Je suis la page d'accueil"/>
          <button text="Liste de Films" @tap="onButtonListTap"/>
          <button text="Liste de amis" @tap="onFriendListTap"/>
        </StackLayout>
            
    </Page>
</template>

<script >
import movieslistpage from "./MoviesListPage";
import friendlist from "./FriendList";
import settings from "./Setting";
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
    onSettingsTap: function(event){
      this.$navigateTo(settings)
    }
  }
  }
  

</script>

<style scoped>
    ActionBar {
      
        background-color: #53ba82;
        color: #ffffff;
    }

.actionbarTitle{
text-align: center;
font-size: 20px;
font-weight: bold;
font-family: sans-serif;
}
    .message {
        vertical-align: center;
        text-align: center;
        font-size: 20;
        color: #333333;
    }
</style>

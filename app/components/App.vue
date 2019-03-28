<template>
    <Page @loaded="pageLoaded">
        <ActionBar >
          <label class="actionbarTitle" text="Netfilm"/>
            <ActionItem text="Settings" android.position="popup" android.systemIcon="ic_menu_more" @tap="onSettingTap"/>
            <ActionItem text="Extra" android.position="popup" android.systemIcon="ic_menu_more" @tap="onExtraTap"/>
        </ActionBar>
        <StackLayout verticalAlignment="center">
          <button text="Liste de Films" @tap="onButtonListTap" class="button"/>
          <button text="Liste des films prêtés" @tap="onSharedListTap" class="button"/>
          <button text="Liste de amis" @tap="onFriendListTap" class="button"/>
        </StackLayout>

    </Page>
</template>

<script >
import movieslistpage from "./MoviesListPage";
import friendlist from "./FriendList";
import sharedmovies from "./SharedMovies";
import extra from "./Form";
import settings from "./setting";
import * as http from "http";
import * as app from 'tns-core-modules/application'
import * as platform from 'tns-core-modules/platform'
import * as color from 'tns-core-modules/color'

  export default {
      data() {
          return {
              fullMovieList: []
          }
      },
      methods: {
          onButtonListTap: function () {
              this.$navigateTo(movieslistpage)
          },
          onFriendListTap: function () {
              this.$navigateTo(friendlist);
          },

          onSharedListTap: function () {
              this.$navigateTo(sharedmovies)
          },

          onExtraTap: function () {
              this.$navigateTo(extra)
          },
          onSettingTap: function () {
              this.$navigateTo(settings);
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

    .button{
        width: 60%;
        horiz-align: center;
    }

    .message {
        vertical-align: center;
        text-align: center;
        font-size: 20px;
        color: #333333;
    }
</style>

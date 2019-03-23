<template>
    <Page>
        <ActionBar >
          <label class="actionbarTitle" text="NetFilm"/>
          <NavigationButton text="Go Back" android.systemIcon="ic_menu_back" @tap="onBackPressed"/>
          <ActionItem android.position="actionBar" android.systemIcon="ic_menu_add" @tap="onAddFriendTap"/>
        </ActionBar>
        <StackLayout orientation="vertical">
            <Label class="header" text="Je suis la page de liste d'amis/es"/>
            <ListView class="listview" for="item in friendlist" @itemTap="friendTap">
                <v-template>
                    <StackLayout orientation="vertical">
                        <Label :text="item.prenom + ' ' + item.nom" class="friendLabel" textWrap="true"/>
                    </StackLayout>
                </v-template>
            </ListView>
        </StackLayout>
            
    </Page>
</template>

<script >
import home from "./App";
import dialogFragment from "./AddFriend";
import friendDetail from "./FriendDetail"
import * as http from "http";

  export default {
      mounted() {

          http.getJSON("http://pam-api.duckdns.org:1337/kevamis").then(
              result => {
                  this.friendlist = result;
              }, error => {

              }
          );
      },
      data() {
          return {
              friendlist: []
          }
      },
      methods: {
          onBackPressed: function (event) {
              this.$navigateTo(home);
          },
          onAddFriendTap: function (event) {
              this.$navigateTo(dialogFragment)

          },
          friendTap: function ({index, e}) {
              this.$navigateTo(friendDetail, {
                  props: {
                      friend: this.friendlist[index]
                  }
              })
          }

      }
  }
  

</script>

<style scoped>
.header {
  font-size: 20vw;
  padding: 10px;
  text-align: center;
}

    .friendLabel {
  font-size: 15vw;
  padding: 10px;
}
.listview{
  height: 100%;
}
</style>

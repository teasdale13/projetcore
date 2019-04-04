<template>
    <Page>
        <ActionBar >
          <label class="actionbarTitle" text="AMIS"/>
          <NavigationButton text="Go Back" android.systemIcon="ic_menu_back" @tap="onBackPressed"/>
          <ActionItem android.position="actionBar" android.systemIcon="ic_menu_add" @tap="onAddFriendTap"/>
        </ActionBar>
        <StackLayout orientation="vertical" >
            <Label class="header" text="Je suis la page de liste d'amis/es"/>
            <ListView class="listview" for="item in friendlist" @itemTap="friendTap" >
                <v-template>
                    <StackLayout orientation="horizontal" class="listviewcell" vertictalAlignment="center">
                        <Label :text="item.prenom + ' ' + item.nom" class="friendLabel" textWrap="true" />
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
  	props: ["allMyFriendAreHere"],

      mounted() {

			this.friendlist = this.allMyFriendAreHere;
			// Va chercher tous les amis dans la base de données.
			http.getJSON("https://pam-api.duckdns.org/kevamis").then(
				result => {
					this.friendlist = result;
				}, error => {
				}
			);
      },
      data() {
          return {
              friendlist: [],
          }
      },
      methods: {
          onBackPressed: function () {
              this.$navigateTo(home);
          },
          onAddFriendTap: function () {
              this.$navigateTo(dialogFragment)

          },
          friendTap: function ({index, e}) {
          	console.log(JSON.stringify(this.friendlist[index]));
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
    height: 50vw;
    padding: 15vw;

}
.listview{
  height: 100%;
}
</style>

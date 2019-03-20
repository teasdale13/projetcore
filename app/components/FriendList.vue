<template>
    <Page>
        <ActionBar >
          <label class="actionbarTitle" text="NetFilm"/>
          <NavigationButton text="Go Back" android.systemIcon="ic_menu_back" @tap="onBackPressed"/>
          <ActionItem android.position="actionBar" android.systemIcon="ic_menu_add" @tap="onAddFriendTap"/>
        </ActionBar>
        <StackLayout orientation="vertical">
            <Label class="header" text="Je suis la page de liste d'amis/es"/>
            <ListView class="listview" for="item in friendlist"  @tap="onItemTap">
                <v-template>
                    <StackLayout orientation="vertical">
                        <Label :text="item.friend" class="friendLabel" textWrap="true"/>
                    </StackLayout>
                </v-template>
            </ListView>

            <button class="addbutton" text="Ajouter un ami" @tap="onAddFriendTap"/>
        </StackLayout>
            
    </Page>
</template>

<script >
import home from "./App";
import dialogFragment from "./AddFriend";
  export default {
    data() {
      return {
        friendlist: [
        { friend: "Ami 1..... C'est qui déjà!?" },
        { friend: "Ami 2..... Quand même étonnant que j'en aille autant!" },
        { friend: "Ami 3..... Foutaise!" },
        { friend: "Ami 4..... Je dois surement rêver!" },
        { friend: "Ami 5..... Ce n'est plus drôle!" },
        { friend: "Ami 6..... C'est clairement pour l'argent!" },
      ]
      }
    },
    methods: {
    onBackPressed: function(event) {
      this.$navigateTo(home);
    },
    onAddFriendTap: function(event){
        this.$showModal(dialogFragment);
     /* var dialogs = require("tns-core-modules/ui/dialogs");

      const promptOptions = {
        title: "Ajouter un ami",
        message: "Veuillez inscrire le nom",
        okButtonText: "Ok",
        cancelButtonText: "Cancel",
        defaultText: "",
        inputType: "text",
        capitalizationType: "sentences"
    };
      prompt(promptOptions).then((r) => {
          if(r.text !== ""){
            this.friendlist.push({friend: r.text});
          }
      });*/

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

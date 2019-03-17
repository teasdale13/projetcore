<template>
    <Page>
        <ActionBar >
          <label class="actionbarTitle" text="NetFilm"/>
          <NavigationButton text="Go Back" android.systemIcon="ic_menu_back" @tap="onBackPressed"/>
        </ActionBar>
        <StackLayout>
          <Label class="message" text="Settings"/>
          <TextField id="firstname" hint="Prénom" returnKeyType="done" :text="firstname"/>
          <TextField id="lastname" hint="Nom" returnKeyType="done" :text="lastname"/>
          <button text="Ajouter un ami" @tap="onAddFriendTap"/>

          <Label :text="fullName" class="fullName"/>
        </StackLayout>
            
    </Page>
</template>

<script >
import home from "./App";
  export default {
    data() {
      return {
          firstname: '',
          lastname: '',
          fullName: ''
      }
    },
    methods: {
    onBackPressed: function(event) {
      this.$navigateTo(home);
    },
    onAddFriendTap: function(){ 
        pageLoaded();
        function pageLoaded(args) {
            var view = require("ui/core/view");
            var page = args.object;
            var firstName = view.getViewById(page, "firstname");
            var lastName = view.getViewById(page, "lastname");
        }
        this.firstname = firstName;
        this.lastname = lastName;
        var dialogs = require("tns-core-modules/ui/dialogs");
        dialogs.alert({
            title: "Vous avez ajouté",
            message: firstName + " " + lastName,
            okButtonText: "Confirmer"
        }).then(function (r) {
        console.log("Dialog closed!");
            this.fullName = firstName + " " + lastName;
        });

        
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
    .fullName{
        font-size: 20;
        color: #333333;
    }
</style>

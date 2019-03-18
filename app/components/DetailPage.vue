<template>
    <Page>
        <ActionBar>
            <label class="actionbarTitle" text="NetFilm"/>
             <NavigationButton text="Go Back" android.systemIcon="ic_menu_back" @tap="onBackPressed"/>
        </ActionBar>
        <StackLayout>
          <Label class="message" text="Je suis la page de details"/>
            <Label :text="`Le film est prêté à `+ nom" class="text"/>
          <Button text="Prêter le film"  @tap="onButtonSharedTap"/>
        </StackLayout>
            
    </Page>
</template>

<script >
import movieslistpage from "./MoviesListPage";

  export default {
    data() {
      return {
        nom: ""
      }
    },
    methods: {
    onBackPressed: function(event){
        this.$navigateTo(movieslistpage);
    },

    onButtonSharedTap: function (event) {

        const promptOptions = {
            message: "À qui le prêtez-vous?",
            cancelButtonText: "Cancel",
            actions: ["Maurice",
                "Fardoche",
                "Passe-Carreau",
                "Passe-Montagne",
                "Jean Charest",
                "Yoda",
                "L'armée du salut"]

        };
        action(promptOptions).then((r) => {
            console.log("Dialog result: " + r);
            this.nom = r.toString();

        });

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
    .text{
        font-size: 20px;
        padding: 15px;
        text-align: center;
    }
</style>

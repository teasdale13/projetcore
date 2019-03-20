<template>
    <Page>
        <ActionBar>
            <label class="actionbarTitle" text="NetFilm"/>
             <NavigationButton text="Go Back" android.systemIcon="ic_menu_back" @tap="onBackPressed"/>
        </ActionBar>
        <StackLayout>
          <Label class="message" text="myMovie.nom"/>
            <Label :text="`Le film est prêté à `+ myMovie.kevamis" class="text"/>
            <!--<Image :src="METTRE URL DE L'IMAGE ICI"/> -->
          <Button text="Prêter le film"  @tap="onButtonSharedTap"/>
        </StackLayout>
            
    </Page>
</template>

<script >
    import * as http from "http";
    import movieslistpage from "./MoviesListPage";

  export default {
      props: ["movie"],
    data() {
      return {
        myMovie: []
      }
    },
      mounted(){
          http.getJSON("http://pam-api.duckdns.org:1337/kevfilms/1").then(
              result => {
                  this.myMovie = result;
                  console.log("HTTP GET JSON = " + JSON.stringify(this.myMovie));
          })

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

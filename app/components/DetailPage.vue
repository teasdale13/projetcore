<template>
    <Page>
        <ActionBar>
            <label class="actionbarTitle" text="NetFilm"/>
             <NavigationButton text="Go Back" android.systemIcon="ic_menu_back" @tap="onBackPressed"/>
        </ActionBar>
        <StackLayout>
          <Label class="message" :text="this.myMovie.titre"/>
            <Label :text="`Le film est prêté à `+ this.kevamisArray.nom" class="text"/>
            <Label :text="this.myMovie.titre" class="text"/>
            <!--<Image :src="METTRE URL DE L'IMAGE ICI"/> -->
          <Button text="vkhgvghkvjkghkgvjhkgvkjhgvhkgjvjhgfjhgvjhgvjgv"  @tap="onButtonSharedTap"/>
        </StackLayout>
            
    </Page>
</template>

<script >
    import * as http from "http";
    import movieslistpage from "./MoviesListPage";

  export default {
      props: ["movie", "prenom"],

    data() {
      return {
        myMovie: [],
          kevamisArray: [],
      }
    },
      mounted(){
          http.getJSON("http://pam-api.duckdns.org:1337/kevfilms/" + this.movie.id.toString()).then(
              result => {
                  this.myMovie = result;
                  console.log("HTTP GET JSON1 = " + JSON.stringify(this.myMovie));
                  this.kevamisArray = this.myMovie.kevamis;
                  console.log("HTTP GET JSON2 = " + JSON.stringify(this.kevamisArray));
                  console.log("HTTP GET JSON3 = " + JSON.stringify(this.kevamisArray[0].nom));
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

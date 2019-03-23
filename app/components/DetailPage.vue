<template>
    <Page>
        <ActionBar>
            <label class="actionbarTitle" text="NetFilm"/>
             <NavigationButton text="Go Back" android.systemIcon="ic_menu_back" @tap="onBackPressed"/>
        </ActionBar>

        <ScrollView>
        <StackLayout>
          <Label class="message" :text="this.myMovie.titre"/>

            <Label :text="this.myMovie.titre" class="text"/>
            <Label :text="this.myMovie.duree" class="text"/>
            <Label :text="this.myMovie.scenario" class="text"/>
            <Label :text="this.myMovie.anneesortie" class="text"/>
            <Label text="Ceux qui ont emprunté le film :" class="text"/>
            <ListView for="item in kevamisArray"  @itemTap="onItemTap">
                <v-template>
                    <StackLayout orientation="vertical">
                        <Label :text="item.nom" class="movieLabel" textWrap="true"/>
                    </StackLayout>
                </v-template>
            </ListView>

            <Label text="Type :" class="text"/>

            <ListView for="item in kevtypesArray"  @itemTap="onItemTap" class="typelv">
                <v-template>
                    <StackLayout orientation="vertical">
                        <Label :text="item.description" class="movieLabel" textWrap="true"/>
                    </StackLayout>
                </v-template>
            </ListView>
            <Image src="https://i.imgur.com/x3QWpLs.jpg" class="image"/>
          <Button text="Prêter le film"  @tap="onButtonSharedTap"/>
        </StackLayout>
        </ScrollView>
            
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
          kevanneArray: [],
          kevtypesArray: [],
      }
    },
      mounted(){
          http.getJSON("http://pam-api.duckdns.org:1337/kevfilms/" + this.movie.id.toString()).then(
              result => {
                  this.myMovie = result;
                  console.log("HTTP GET JSON1 = " + JSON.stringify(this.myMovie));
                  this.kevamisArray = this.myMovie.kevamis;
                  this.kevanneArray = this.myMovie.kevannee;
                  if (this.myMovie.kevtypes != null){
                      this.kevtypesArray = this.myMovie.kevtypes;
                  } else{
                      this.kevtypesArray = null;
                  }

                  console.log("HTTP GET JSON2 = " + JSON.stringify(this.kevamisArray));
                  console.log("HTTP GET JSON3 = " + JSON.stringify(this.kevamisArray[0].nom));
                  console.log("HTTP GET JSON4 = " + JSON.stringify(this.kevtypesArray));
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

    .image{
        height: 500px;
        width: 400px;
    }

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

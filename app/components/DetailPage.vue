<template>
    <Page>
        <ActionBar>
            <label class="actionbarTitle" text="FILM EN DÉTAILS"/>
             <NavigationButton text="Go Back" android.systemIcon="ic_menu_back" @tap="onBackPressed"/>
        </ActionBar>

        <ScrollView height="100%">

        <StackLayout class="container">
            <Label :text="`Titre : ` + this.myMovie.titre" class="title" textWrap="true"/>
            <StackLayout orientation="horizontal" >
                <Image src="https://i.imgur.com/x3QWpLs.jpg" class="image"/>
                <StackLayout orientation="vertical" class="blabla">

                    <Label :text="`Année : ` + this.Myannee" class="text" textWrap="true"/>
                    <Label :text="`Durée : ` + this.myMovie.duree + ` minutes`" class="text" textWrap="true"/>

                <StackLayout orientation="horizontal">
                    <Label text="Type : " class="type"/>
                    <ListView for="item in kevtypesArray" class="typelv">
                        <v-template>
                            <StackLayout orientation="vertical">
                                <Label :text="item.description" class="movieLabel" textWrap="true"/>
                            </StackLayout>
                        </v-template>
                    </ListView>
                </StackLayout>
                </StackLayout>
            </StackLayout>

            <Label :text="`Scénario : ` + this.myMovie.scenario" class="text" textWrap="true"/>
            <Label text="Ceux qui ont emprunté le film :" class="text"/>
            <ListView for="item in kevamisArray">
                <v-template>
                    <StackLayout orientation="vertical">
                        <Label :text="item.nom" class="movieLabel" textWrap="true"/>
                    </StackLayout>
                </v-template>
            </ListView>

            <StackLayout orientation="horizontal" >
                <Button text="Prêter"  @tap="onButtonSharedTap" width="50%" class="button"/>
                <Button text="Supprimer" @tap="onDeleteTap" width="50%" class="button"/>
            </StackLayout>
        </StackLayout>
        </ScrollView>
            
    </Page>
</template>

<script >
    import * as http from "http";
    import movieslistpage from "./MoviesListPage";

  export default {
      props: ["movie"],

    data() {
      return {
        myMovie: [],
          kevamisArray: [],
          kevanneArray: [],
          kevtypesArray: [],
          fullnameList: [],
          sharedList: [],
          Myannee: null,
          fullMovieList: []
      }
    },
      mounted(){
          http.getJSON("http://pam-api.duckdns.org:1337/kevfilms/" + this.movie.id.toString()).then(
              result => {
                  this.myMovie = result;
                  http.getJSON("http://pam-api.duckdns.org:1337/kevannees/" + this.myMovie.anneesortie.toString()).then(
                      result => {
                          console.log("ANNEE " + JSON.stringify(result));
                          this.Myannee = result.annee.toString();
                      }
                  );
                  console.log("HTTP GET JSON1 = " + JSON.stringify(this.myMovie));
                  this.kevamisArray = this.myMovie.kevamis;
                  this.kevanneArray = this.myMovie.kevannee;
                  if (this.myMovie.kevtypes != null){
                      this.kevtypesArray = this.myMovie.kevtypes;
                  } else{
                      this.kevtypesArray = null;
                  }
          });



      },
    methods: {
    onBackPressed: function(event){
        this.$navigateTo(movieslistpage);
    },

    onButtonSharedTap: function (event) {

        http.getJSON("http://pam-api.duckdns.org:1337/kevamis").then(
            result => {
                this.sharedList = result;
                console.log(JSON.stringify(this.sharedList.length));
                for (var x = 0; x < this.sharedList.length; x++){
                    this.fullnameList.push((this.sharedList[x].prenom).concat(" " + this.sharedList[x].nom) );
                }
                console.log(JSON.stringify(this.fullnameList));

                const promptOptions = {
                    message: "À qui le prêtez-vous?",
                    cancelButtonText: "Cancel",
                    actions: this.fullnameList

                };
                action(promptOptions).then((r) => {
                    console.log("Dialog result: " + r);
                    this.nom = r.toString();

                });
            }
        );

    },
        onDeleteTap: function () {

           http.request({
                url: "http://pam-api.duckdns.org:1337/kevfilms/" + this.movie.id.toString(),
                method: "DELETE"
            }).then((response) => {

            }, (e) => {
            });

           http.getJSON("http://pam-api.duckdns.org:1337/kevfilms").then(
               result => {
                   this.fullMovieList = result;
               }
           ).then(
               response => {
                   console.log(JSON.stringify(response));
               }
           );

            this.$navigateTo(movieslistpage, {
                props: {
                    allMovies: this.fullMovieList
                }
            });
        }
}
  }
  

</script>

<style scoped>

    .container{
        padding: 10px;
    }
    .image{
        height: 30%;
        width: 30%;
        padding: 15px;
        margin: 10px;
    }
    .type{
        font-size: 20px;
        margin: auto;
    }
    .title{
        padding: 15px;
        text-align: center;
        font-size: 20px;
    }
    .typelv{
        height: 150px;
    }
    .movieLabel{
        text-align: center;
    }
    .blabla{
        vertical-align: auto;
    }
    .text{
        font-size: 15px;
        margin: auto;
    }
    .button{
        padding: 5px;
    }
</style>

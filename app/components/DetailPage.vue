<template>
    <Page>
        <ActionBar>
            <label class="actionbarTitle" text="NetFilm"/>
             <NavigationButton text="Go Back" android.systemIcon="ic_menu_back" @tap="onBackPressed"/>
        </ActionBar>

        <ScrollView height="100%">

        <StackLayout>
            <Label :text="`Titre : ` + this.myMovie.titre" class="title" textWrap="true"/>
            <StackLayout orientation="horizontal" >
                <Image src="https://i.imgur.com/x3QWpLs.jpg" class="image"/>
                <StackLayout orientation="vertical" width="40%" class="blabla">

                    <Label :text="`Année : ` + this.myMovie.anneesortie" class="text" textWrap="true"/>
                    <Label :text="`Durée : ` + this.myMovie.duree" class="text" textWrap="true"/>

                <StackLayout orientation="horizontal">
                    <Label text="Type : " class="type" width="35%%"/>
                    <ListView for="item in kevtypesArray"  @itemTap="onItemTap" class="typelv">
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
            <ListView for="item in kevamisArray"  @itemTap="onItemTap">
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
          sharedList: []
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
          })

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

           /* http.request({
                url: "http://pam-api.duckdns.org:1337/kevfilms/" + this.movie.id.toString(),
                method: "DELETE"
            }).then((response) => {

            }, (e) => {
            });*/
            this.$navigateTo(movieslistpage);
        }
}
  }
  

</script>

<style scoped>

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
        width: 30%;
    }
    .movieLabel{
        text-align: center;
    }
    .blabla{
        vertical-align: auto;
    }
    .text{
        font-size: 20px;
        margin: auto;
    }
    .button{
        padding: 5px;
    }
</style>

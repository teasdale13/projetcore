<template>
  <Page>
    <ActionBar>
        <label class="actionbarTitle" text="NetFilm"/>
      <NavigationButton text="Go Back" android.systemIcon="ic_menu_back" @tap="onBackPressed"/>
      <ActionItem android.position="actionBar" text="Ajouter" android.systemIcon="ic_menu_add" @tap="onAddTap"/>
    </ActionBar>
    <StackLayout orientation="vertical">
      <Label class="header" text="Je suis la page de liste de films"/>
      <ListView for="item in listMovieItem"  @itemTap="onItemTap">
        <v-template>
          <StackLayout orientation="vertical">
            <Label :text="item.movie" class="movieLabel" textWrap="true"/>
          </StackLayout>
        </v-template>
      </ListView>
    </StackLayout>
  </Page>
</template>

<script >
import * as http from "http";
import home from "./App";
import detail from "./DetailPage";
import sharedlist from "./SharedMovies";
import form from "./Form";
export default {
  props: [],

  mounted(){
    // Va chercher la liste de films à partir d'un URL.
    http.getJSON("http://pam-api.duckdns.org:1337/kevfilms").then(
       result =>{
         this.listMovieItem = result.results;

    },
    error => {
         console.log("ERREUR" + error);
    });



  },
  data() {
    return {
      listMovieItem: [],

      itemList: [
        { movie: "Je suis une cellule parmis d'autre cellules" },
        { movie: "Je suis une cellule parmis d'autre cellules" },
        { movie: "Je suis une cellule parmis d'autre cellules" },
        { movie: "Je suis une cellule parmis d'autre cellules" },
        { movie: "Je suis une cellule parmis d'autre cellules" },
        { movie: "Je suis une cellule parmis d'autre cellules" }
      ]
    };
  },
  methods: {
    onBackPressed: function(event) {
      this.$navigateTo(home);
    },
    onItemTap: function({index, e}){
        console.log("onItemTap");
        this.$navigateTo(detail, {
          props: {
            movieTap: this.listMovieItem[index]
          }
        });
    },
    onAddTap: function(event){
        this.$navigateTo(form);
    },
    onSharedTap: function(event){
        this.$navigateTo(sharedlist);
    }
    
  }
};
</script>

<style scoped>


.header {
  font-size: 20vw;
  padding: 10px;
  text-align: center;
}
</style>

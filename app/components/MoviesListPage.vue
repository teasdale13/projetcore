<template>
  <Page xmlns:ui="nativescript-menu">
    <ActionBar>
        <label class="actionbarTitle" text="NetFilm"/>
      <NavigationButton text="Go Back" android.systemIcon="ic_menu_back" @tap="onBackPressed"/>
      <ActionItem android.position="popup" text="Ajouter" icon="res://menu_white" @tap="onAddTap"/>
      <ActionItem android.position="popup" text="Ajouter OMDB" icon="res://menu_white" @tap="onOMDBTap"/>
    </ActionBar>
    <StackLayout orientation="vertical">
      <Label class="header" text="Je suis la page de liste de films"/>
      <ListView for="item in itemList"  @tap="onItemTap">
        <v-template>
          <StackLayout orientation="vertical">
            <Label :text="item.movie" class="movieLabel"/>
          </StackLayout>
        </v-template>
      </ListView>
    </StackLayout>
  </Page>
</template>

<script >
import home from "./App";
import list from "./MoviesListPage";
import detail from "./DetailPage";
import omdb from "./FormOMDB";
import form from "./Form";
export default {
  data() {
    return {
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
    onItemTap: function(event){
        console.log("onItemTap");
        this.$navigateTo(detail);
    },
    onAddTap: function(event){
        this.$navigateTo(form);
    },
    onOMDBTap: function(event){
        this.$navigateTo(omdb);
    }
    
  }
};
</script>

<style scoped>
ActionBar {

    text-align: center;
  background-color: #53ba82;
  color: #ffffff;
}
.actionbarTitle{
text-align: center;
font-size: 20px;
font-weight: bold;
font-family: sans-serif;
}
.movieLabel {
  font-size: 15vw;
}

.header {
  font-size: 20vw;
  padding: 10px;
  text-align: center;
}
</style>

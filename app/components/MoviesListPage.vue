<template>
  <Page xmlns:ui="nativescript-menu">
    <ActionBar>
        <label class="actionbarTitle" text="NetFilm"/>
      <NavigationButton text="Go Back" android.systemIcon="ic_menu_back" @tap="onBackPressed"/>
      <ActionItem android.position="actionBar" text="Ajouter" android.systemIcon="ic_menu_add" @tap="onAddTap"/>
    </ActionBar>
    <StackLayout orientation="vertical">
      <Label class="header" text="Je suis la page de liste de films"/>
      <ListView for="item in itemList"  @tap="onItemTap">
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
import home from "./App";
import list from "./MoviesListPage";
import detail from "./DetailPage";
import omdb from "./SharedMovies";
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


.header {
  font-size: 20vw;
  padding: 10px;
  text-align: center;
}
</style>

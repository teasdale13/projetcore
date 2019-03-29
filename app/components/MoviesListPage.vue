<template>
    <Page @loaded="onLoaded">
        <ActionBar>
            <label class="actionbarTitle" text="FILMS"/>
            <NavigationButton text="Go Back" android.systemIcon="ic_menu_back" @tap="onBackPressed"/>
            <ActionItem android.position="actionBar" text="Ajouter" android.systemIcon="ic_menu_add" @tap="onAddTap"/>
        </ActionBar>
        <StackLayout orientation="vertical">
            <SearchBar hint="Search" id="searchbar"  @textChange="onTextChanged" @submit="onSubmit" @clear="onClear" />
            <Label class="header" text="Je suis la page de liste de films"/>
            <ListView for="item in listMovieItem"  @itemTap="onItemTap" height="100%">
                <v-template>
                    <StackLayout orientation="horizontal" class="listviewcell" >
                        <Label :text="item.titre" class="movieLabel" textWrap="true"/>
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
    import addmovie from "./AddMovie";
    export default {
        props: ["titre", "allMovies"],

        mounted(){
            this.listMovieItem = this.allMovies;

            console.log("mounted");
            // Va chercher la liste de films à partir d'un URL.
            http.getJSON("https://pam-api.duckdns.org/kevfilms").then(
                result => {
                    this.listMovieItem = result;
                    console.log("STRING OF LIST" + JSON.stringify(result));
                },
                error => {
                    console.log("ERREUR" + error);
                });

            /* Va chercher toutes les années et créer 2 tableaux. Un qui sert à garder toutes l'informations
             * de l'objet année et un autre qui contient seulement l'année en STRING pour l'afficher dans le
             * ListPicker dans la page  AddMovie.vue */
            http.getJSON("https://pam-api.duckdns.org/kevannees").then(
                result => {
                    this.anneeArray = result;
                    console.log("ANNEE " + JSON.stringify(this.anneeArray));
                    for (var x = 0; x < this.anneeArray.length; x++){
                        this.anneeAsNumber.push(this.anneeArray[x].annee.toString());
                    }
                }, error => {
                    console.log("ERREUR" + error);
                }
            );
        },
        data() {
            return {
                listMovieItem: [],
                anneeArray: [],
                anneeAsNumber: [],
                page: null

            };
        },
        methods: {
            onSubmit:function(){
            var view = require("ui/core/view");
            var recherche = view.getViewById(this.page, "searchbar").text.toString();
            var myItems = [];
                if (recherche !== "") {
                    for (let i = 0; i < this.listMovieItem.length; i++) {
                        if (this.listMovieItem[i].titre.toLowerCase().includes(recherche)) {
                            myItems.push(this.listMovieItem[i]);
                        }
                    }
                }

                this.listMovieItem = myItems;
            },
            onClear: function(){
                http.getJSON("https://pam-api.duckdns.org/kevfilms").then(
                    result => {
                        this.listMovieItem = result;
                        console.log("STRING OF LIST" + JSON.stringify(result));
                    },
                    error => {
                        console.log("ERREUR" + error);
                    });
            },
            onBackPressed: function() {
                this.$navigateTo(home);
            },
            onItemTap: function({index, e}){
                console.log("onItemTap");
                this.$navigateTo(detail, {
                    props: {
                        movie: this.listMovieItem[index],

                    }
                });
                console.log("MOVIE " + this.listMovieItem[index])
            },

            onAddTap: function(){
                this.$navigateTo(addmovie, {
                    props: {
                        annees: this.anneeAsNumber,
                        myAnneeArray: this.anneeArray

                    }
                });
            },
            onLoaded: function (args) {
                this.page = args.object;
                console.log("onLoaded");
            }

        }
    };
</script>

<style scoped>

    .movieLabel{
        padding-left: 20vw;
        font-size: 15vw;
        height: 50vw;
        vertical-align: center;
        text-align: center;
    }


    .header {
        font-size: 20vw;
        padding: 10px;
        text-align: center;
    }
</style>

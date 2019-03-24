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
        props: ["titre"],

        mounted(){
            // Va chercher la liste de films à partir d'un URL.
            http.getJSON("http://pam-api.duckdns.org:1337/kevfilms").then(
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
            http.getJSON("http://pam-api.duckdns.org:1337/kevannees").then(
                result => {
                    this.anneeArray = result;
                    console.log("ANNEE " + JSON.stringify(this.anneeArray));
                    for (var x = 0; x < this.anneeArray.length; x++){
                        this.anneeAsNumber.push(this.anneeArray[x].annee.toString());
                        console.log("UN TEST " + JSON.stringify(this.anneeArray[x].annee));
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
                anneeAsNumber: []

            };
        },
        methods: {
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

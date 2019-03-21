<template>
    <Page>
        <ActionBar>
            <label class="actionbarTitle" text="NetFilm"/>
            <NavigationButton text="Go Back" android.systemIcon="ic_menu_back" @tap="onBackPressed"/>
            <ActionItem android.position="actionBar" text="Ajouter" android.systemIcon="ic_menu_add" @tap="onAddTap"/>
        </ActionBar>
        <StackLayout orientation="vertical">
            <Label class="header" text="Je suis la page de liste de films"/>
            <!--<ListView for="film in listMovieItem"  @itemTap="onItemTap">
                <v-template>
                    <StackLayout orientation="vertical">
                        <Label :text="film.titre" class="movieLabel" textWrap="true"/>
                    </StackLayout>
                </v-template>
            </ListView>-->
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
        props: ["titre"],

        mounted(){
            // Va chercher la liste de films à partir d'un URL.
            http.getJSON("http://pam-api.duckdns.org:1337/kevfilms").then(
                result => {
                    this.listMovieItem = result.results;
                    console.log("STING OF LIST" + JSON.parse(this.listMovieItem));

                },
                error => {
                    console.log("ERREUR" + error);
                });

        },
        data() {
            return {
                listMovieItem: []

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

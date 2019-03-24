<template>
    <Page @loaded="pageLoaded">
        <ActionBar>
            <label class="actionbarTitle" text="NetFilm"/>
            <NavigationButton text="Go Back" android.systemIcon="ic_menu_back" @tap="onBackPressed"/>
        </ActionBar>
        <StackLayout>
            <Label class="message" text="Formulaire"/>

            <Button text="Take a picture" @tap="takePhoto"/>
            <!--<Image :src="img" width="200" height="200"/>-->
            <TextField hint="Titre" />
            <TextField hint="Scénario" />
            <TextField hint="Durée en minutes" keyboardType="number"/>
            <StackLayout orientation="horizontal">
                <StackLayout orientation="vertical" width="50%">
                    <Button  text="Type" @tap="addMovieType"/>
                    <ListView for="item in movieType"  @itemTap="onItemTap">
                        <v-template>
                            <StackLayout orientation="vertical">
                                <Label :text="item.description" class="movieLabel" textWrap="true"/>
                            </StackLayout>
                        </v-template>
                    </ListView>
                </StackLayout>

                    <ListPicker :items="anneeAsNumber" id="listpicker" width="100%"/>

            </StackLayout>

            <Button text="Sauvegarder" @tap="saveToDB" />


        </StackLayout>

    </Page>
</template>

<script>
    import {Image} from "tns-core-modules/ui/image";
    import * as camera from "nativescript-camera";
    import movieslistpage from "./MoviesListPage";
    import * as http from "http";

    export default {
        props:["annees","myAnneeArray"],

        mounted(){
            console.log("MOUNTED " + this.annees);
            this.anneeAsNumber = this.annees;
            http.getJSON("http://pam-api.duckdns.org:1337/kevtypes").then(
                result => {
                    this.allTypes = result;
                    for (var x = 0; x < this.allTypes.length; x++){
                        this.allTypesString.push(this.allTypes[x].description);
                        console.log("types " + this.allTypesString[x]);
                    }
                }
            )

        },

        data() {
            return{
                img: "",
                anneeArray: this.myAnneeArray,
                anneeAsNumber: this.annees,
                allTypes: [],
                movieType: [],
                allTypesString: []
            }

        },
        methods: {

            takePhoto() {
                camera.requestPermissions().then(() => {
                    camera.takePicture({width: 300, height: 300, keepAspectRatio: true, saveToGallery: true})
                        .then((imageAsset) => {
                            var image = new Image();
                            image.src = imageAsset;
                            this.img = imageAsset;
                        })
                        .catch(e => {
                            console.log('error:', e);
                        });
                })
                    .catch(e => {
                        console.log('Error requesting permission');
                    });

            },
            pageLoaded: function(args){
                console.log("PAGE LOADED");
                this.page = args.object;
                console.log("TEST pageLoaded " + this.page.toString());
            },
            addMovie: function(){
                http.request({
                    url: "http://pam-api.duckdns.org:1337/kevfilms",
                    method: "POST",
                    headers: {"Content-Type": "application/json"},
                    content: JSON.stringify({

                    })
                }).then((response) => {
                    // vérifier le retour pour en attribuer le comportement.
                }, (e) => {
                    // trapper les erreurs ici.
                })
            },
            onBackPressed: function (event) {
                this.$navigateTo(movieslistpage);
            },

            saveToDB: function () {
                var view = require("ui/core/view");
                var year = view.getViewById(this.page, "listpicker");
                console.log("YEAR : " + this.anneeArray[year.selectedIndex].id);

            },
            addMovieType: function () {

            }

        }


    }

</script>

<style scoped>


</style>
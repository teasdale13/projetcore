<template>
    <Page>
        <StackLayout>
            <ActionBar>
                <label class="actionbarTitle" text="NetFilm"/>
                <NavigationButton text="Go Back" android.systemIcon="ic_menu_back" @tap="onBackPressed"/>
            </ActionBar>
            <Label class="message" text="Formulaire"/>

            <Button text="Take a picture" @tap="takePhoto"/>
            <Image :src="img" width="200" height="200"/>

        </StackLayout>

    </Page>
</template>

<script>
    import {Image} from "tns-core-modules/ui/image";
    import * as camera from "nativescript-camera";
    import movieslistpage from "./MoviesListPage";

    export default {

        data() {
            img: ""
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
            addMovie: function(){
                http.request({
                    url: "METTRE URL ICI",
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
        }


    }

</script>

<style scoped>


</style>
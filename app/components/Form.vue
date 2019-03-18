<template>
    <Page>
        <ActionBar>
            <label class="actionbarTitle" text="NetFilm"/>
            <NavigationButton text="Go Back" android.systemIcon="ic_menu_back" @tap="onBackPressed"/>
        </ActionBar>
        <StackLayout>
            <Label class="message" text="Formulaire"/>
            <Button text="Take a picture" @tap="takePhoto"/>
            <Image :src="img" width="200" height="200"/>

            <Label class="message" text="Formulaire OMDB"/>
            <Label class="message" text="Une partie de L'Osti D'jeu!?"/>
            <Label class="question" :text="randomQuestion" textWrap="true"/>
            <Label class="response" :text="randomResponse" textWrap="true"/>
            <Button text="Question" @tap="questionTap"/>
            <Button text="Réponse" @tap="responseTap"/>
        </StackLayout>

    </Page>
</template>

<script>
    import {Image} from "tns-core-modules/ui/image";
    import * as camera from "nativescript-camera";
    import movieslistpage from "./MoviesListPage";

    export default {
        data() {
            return {
                img: '',
                randomQuestion: '',
                randomResponse:''
            }
        },
        methods: {
            onBackPressed: function (event) {
                this.$navigateTo(movieslistpage);
            },
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
            questionTap: function(event){
                var questions = ["Faire du pouce avec ...", "La première chose que l'ancien tueur fait en sortant de prison c'est ...",
                    "Mon vote pour le trou de cul du monde va à : ...", "Faire garder ses enfants par ...", "C'est quoi le meilleur truc contre les lendemains de veille? ...",
                    "C'est quoi ton parfum ? ...", "Flatte ...", "Qu'est-ce qui gosse en tabarnak? ...", "Faire un DEP pour apprendre à ...",
                    "En jouant à Pokémon Go, j'ai pogné ...", "C'pas un osti de français qui va me montrer à ...", "Qu'est ce qui mène à une mort certaine?..."]

                var tata = Math.floor(Math.random() * questions.length);

                this.randomQuestion = questions[tata];
                this.randomResponse = '';
            },
            responseTap: function(event){
                var responses = ["Libarté!!!!", "La géneration d'enfants-rois.", "Un Tripotanus.", "Grand-maman la raciste." , "Stromgol.",
                    "Sauter en parachute de l'espace.", "Le capitaine Charles Patenaude.", "Saccager la Baie James.", "Un vagin qui pue fort.",
                    "Un orphelin qui meurt le soir de Noël.", "Mettre les cendres de papi dans le rapas de Noël.", "Me soûler à ma première communion.",
                    "Toute sauf d'l'osti d'root beer.", "Un gros dégueulasse.", "Une danseuse pas de dents de Coaticook devenue pornstar.",
                    "Mon doigt mouillé dans ton oreille.", "Faire des avances à la gardienne.", "Croquer dans un néon.", "Pisser du gaz de schiste.",
                    "Ordonner des génocides.", "Fêter mes 10 de vie commune avec mon ami imaginaire.", "Une fille qui jouit en mettant des insectes sur son clit."]
                if(this.randomQuestion !== ''){
                    this.randomResponse = responses[Math.floor(Math.random() * responses.length)];
                }else{
                    alert("Il faut d'abord tirer un début de phrase!!");
                }

            }
        }
    }


</script>

<style scoped>
    .question{
        color: black;
        text-align: center;
        padding: 15vw;
        font-size: 17vw;
    }

    .response{
        color: red;
        text-align: center;
        padding: 15vw;
        font-size: 17vw;
    }



    .message {
        vertical-align: center;
        text-align: center;
        font-size: 20px;
        color: #333333;
    }
</style>

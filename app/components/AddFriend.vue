<template>
    <Page class="dialog" @loaded="pageLoaded">
        <FlexboxLayout class="stack" flexDirection="column">
            <Label text="Entrez le prénom:" textWrap="true" width="100%"/>
            <TextField hint="Prénom" id="firstname" class="editText" width="100%"/>
            <Label text="Entrez le nom:" textWrap="true" width="100%"/>
            <TextField hint="Nom" class="editText" width="100%" id="lastname"/>
            <Button text="Confirmer" @tap="closeModal"/>

        </FlexboxLayout>
    </Page>
</template>

<script>
    import listFirends from "./FriendList";
    import * as http from "http";


    export default {
        data(){
            return {
                page: null,

            }
        },
        methods: {

            pageLoaded: function (args) {
                this.page = args.object;
                console.log("TEST pageLoaded " + this.page.toString());
            },

            /**
             * Méthode qui ferme la page et qui fait un POST à la base de
             * données pour inséré un nouvel ami.
             *
             */
            closeModal: function () {
                console.log("closeModal Function");
                var view = require("ui/core/view");
                // Assigner le texte qui se retrouve dans le TEXTFIELD avec le ID "firstname".
                var textprenom = view.getViewById(this.page, "firstname").text.toString();
                // Assigner le texte qui se retrouve dans le TEXTFIELD avec le ID "lastname".
                var textnom = view.getViewById(this.page, "lastname").text.toString();

                console.log("TEST " + textprenom + " " + textnom);

                // Appel à la base de donnée et insère un ami (POST)
                http.request({
                    url: "http://pam-api.duckdns.org:1337/kevamis",
                    method: "POST",
                    headers: { "Content-Type": "application/json" },
                    content: JSON.stringify({
                        prenom: textprenom,
                        nom: textnom
                    })
                }).then((response) => {

                }, (e) => {
                });

                    this.$navigateTo(listFirends);

            }

        }


    }
</script>

<style scoped>
    .dialog{
    /* min-width: 500px;*/
    min-height: 600px;
}
.stack{
    width: 100%;
}

</style>
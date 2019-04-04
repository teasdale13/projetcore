<template>
    <Page @loaded="pageLoaded">
        <ActionBar >
            <label class="actionbarTitle" text="AMI"/>
            <NavigationButton text="Go Back" android.systemIcon="ic_menu_back" @tap="onBackPressed"/>
        </ActionBar>
        <StackLayout>
            <TextField :text="this.myFriend.prenom" id="prenom"/>
            <TextField :text="this.myFriend.nom" id="nom" />
            <StackLayout orientation="horizontal">
                <Button text="modifier" @tap="changeMyFriend" class="button"/>
                <Button text="Supprimer" @tap="executeFriend" class="button"/>
            </StackLayout>
        </StackLayout>
    </Page>
</template>

<script>
    import * as http from "http"
    import friendList from "./FriendList"

    export default {
        props: ["friend"],
        mounted(){
        	this.myFriend = this.friend;
        },
        data(){
            return {
                page: null,
                myFriend: []
            }
        },
        methods: {
        	/**
             * Navigue tel un bateau vers la page de liste d'ami.
             */
            onBackPressed: function () {
                this.$navigateTo(friendList);
            },

            /**
             * Fonction qui exécute un ami. De la violence gratuite mais qui fait tellement de bien
             * à l'âme de pouvoir faire du mal sans conséquences!!!
             */
            executeFriend: function () {
                http.request({
                    url: "https://pam-api.duckdns.org/kevamis/" + this.friend.id.toString(),
                    method: "DELETE"
                    }).then((response) => {

                }, (e) => {
                });

                this.$navigateTo(friendList,{
					props: {
						allMyFriendAreHere: this.getAllMyFriend()
					}
				});
            },

            /**
             * Fonction qui fait un update de l'ami.
             */
            changeMyFriend: function () {
                var view = require("ui/core/view");
                var prenom = view.getViewById(this.page, "prenom").text.toString();
                var nom = view.getViewById(this.page, "nom").text.toString();

                http.request({
                    url: "https://pam-api.duckdns.org/kevamis/" + this.friend.id.toString(),
                    method: "PUT",
                    headers: { "Content-Type": "application/json" },
                    content: JSON.stringify({
                        prenom: prenom,
                        nom: nom
                    })
                }).then((response) => {
                    console.log(JSON.stringify(response));
                }, (e) => {
                });

                this.$navigateTo(friendList, {
                	props: {
						allMyFriendAreHere: this.getAllMyFriend()
                    }
                });

            },

			/**
             * Fonction qui va chercher tous les amis dans la base de données.
			 */
			getAllMyFriend: function(){
				http.getJSON("https://pam-api.duckdns.org/kevamis/").then(
					result => {
						return result;
					}
				)

            },
            pageLoaded: function (args) {
                this.page = args.object;
                console.log("TEST pageLoaded " + this.page.toString());
            },
        }
    }
</script>

<style scoped>

</style>
<template>
    <Page @loaded="onLoaded">
        <ActionBar>
            <label class="actionbarTitle" text="FILM EN DÉTAILS"/>
            <NavigationButton text="Go Back" android.systemIcon="ic_menu_back" @tap="onBackPressed"/>
        </ActionBar>

        <ScrollView height="100%">

            <StackLayout class="container">
                <Image src="https://i.imgur.com/x3QWpLs.jpg" class="image"/>

                <StackLayout orientation="vertical">
                    <TextField :text="this.myMovie.titre" class="title" textWrap="true" id="modifTitle"/>
                    <Label :text="`(` + this.Myannee + `)`" class="text" textWrap="true"/>
                    <StackLayout orientation="horizontal">
                        <TextField :text="this.myMovie.duree" class="text" id="modifDuree" textWrap="true"
                                   keyboardType="number" width="13%"/>
                        <Label text=" minutes" class="text" textWrap="true" verticalAlignment="center"/>
                    </StackLayout>

                    <StackLayout orientation="horizontal">
                        <ListView for="item in kevtypesArray" class="typelv">
                            <v-template>
                                <StackLayout orientation="vertical">
                                    <Label :text="item.description" class="movieLabel" textWrap="true"/>
                                </StackLayout>
                            </v-template>
                        </ListView>
                    </StackLayout>
                </StackLayout>

                <TextView :text="this.myMovie.scenario" class="text" id="boringStory" textWrap="true" maxLength="255"/>

                <!--<Label text="Ceux qui ont emprunté le film :" class="text"/>
                <ListView for="item in kevamisArray">
                    <v-template>
                        <StackLayout orientation="vertical">
                            <Label :text="item.nom" class="movieLabel" textWrap="true"/>
                        </StackLayout>
                    </v-template>
                </ListView>-->

                <StackLayout orientation="horizontal">
                    <Button text="Prêter" @tap="onButtonSharedTap" class="button"/>
                    <Button text="Modifier" @tap="onUpdateTap" class="button"/>
                    <Button text="Supprimer" @tap="onDeleteTap" class="button"/>
                </StackLayout>
            </StackLayout>
        </ScrollView>

    </Page>
</template>

<script>
	import * as http from "http";
	import movieslistpage from "./MoviesListPage";

	export default {
		props: ["movie"],

		data() {
			return {
				myMovie: [],
				kevamisArray: [],
				kevanneArray: [],
				kevtypesArray: [],
				fullnameList: [],
				sharedList: [],
				Myannee: null,
				fullMovieList: [],
				page: null
			}
		},
		mounted() {
			http.getJSON("https://pam-api.duckdns.org/kevfilms/" + this.movie.id.toString()).then(
				result => {
					this.myMovie = result;
					http.getJSON("https://pam-api.duckdns.org/kevannees/" + this.myMovie.anneesortie.toString()).then(
						result => {
							console.log("ANNEE " + JSON.stringify(result));
							this.Myannee = result.annee.toString();
						}
					);
					console.log("HTTP GET JSON1 = " + JSON.stringify(this.myMovie));
					this.kevamisArray = this.myMovie.kevamis;
					this.kevanneArray = this.myMovie.kevannee;
					if (this.myMovie.kevtypes != null) {
						this.kevtypesArray = this.myMovie.kevtypes;
					} else {
						this.kevtypesArray = null;
					}
				});


		},
		methods: {
			onBackPressed: function (event) {
				this.$navigateTo(movieslistpage);
			},

			onButtonSharedTap: function (event) {

				http.getJSON("https://pam-api.duckdns.org/kevamis").then(
					result => {
						this.sharedList = result;
						console.log(JSON.stringify(this.sharedList.length));
						for (var x = 0; x < this.sharedList.length; x++) {
							this.fullnameList.push((this.sharedList[x].prenom).concat(" " + this.sharedList[x].nom));
						}
						console.log(JSON.stringify(this.fullnameList));

						const promptOptions = {
							message: "À qui le prêtez-vous?",
							cancelButtonText: "Cancel",
							actions: this.fullnameList

						};
						var fullname = "";
						action(promptOptions).then((r) => {
							console.log("Dialog result: " + r);
							fullname = r.toString();
							for (var x = 0; x < this.sharedList.length; x++) {
								if ((this.sharedList[x].prenom).concat(" " + this.sharedList[x].nom) === fullname) {

									this.sharedMovieToThisFriend(this.sharedList[x].id);
								}
							}


						});
					}
				);

			},

			sharedMovieToThisFriend: function (id) {
				var test = [];
				var test2 = [];
				var statut = {};
				http.getJSON("https://pam-api.duckdns.org/kevamis/" + id.toString()).then(
					result => {
						test = result;
						console.log("RESULT     "+JSON.stringify(test));

						http.getJSON("https://pam-api.duckdns.org/kevfilms/" + this.myMovie.id.toString()).then(
							result => {
								test2 = result.kevamis;
								console.log("Avant PUSH    "+JSON.stringify(test2));
								test2.push(test);
								console.log("APRES PUSH    "+JSON.stringify(test2));

								http.request({
                                    url: "https://pam-api.duckdns.org/kevfilms/" + this.myMovie.id.toString(),
                                    method: "PUT",
                                    headers: {"Content-Type": "application/json"},
                                    content: JSON.stringify({
                                        kevamis: test2,
                                    })
                                }).then((response) => {
                                    console.log(JSON.stringify(response));
                                }, (e) => {
                                });
							}
						);
					}
				);


			},
			onDeleteTap: function () {

				http.request({
					url: "https://pam-api.duckdns.org/kevfilms/" + this.movie.id.toString(),
					method: "DELETE"
				}).then((response) => {

				}, (e) => {
				});

				http.getJSON("https://pam-api.duckdns.org/kevfilms").then(
					result => {
						this.fullMovieList = result;
					}
				).then(
					response => {
						console.log(JSON.stringify(response));
					}
				);

				this.$navigateTo(movieslistpage, {
					props: {
						allMovies: this.fullMovieList
					}
				});
			},
			/**
             * Fonction qui comme son nom le dit update le film lorsque l'utilisateur
             * appuie sur le bouton. Le fonctionnement va conmme suit! => va chercher
             * ce qui se retrouve dans les TextField et le ship via une requête PUT au
             * serveur (Pas l'employé d'un restaurant là!).
			 */
			onUpdateTap: function () {
				var view = require("ui/core/view");

				var title = view.getViewById(this.page, "modifTitle").text.toString();
				var napLength = view.getViewById(this.page, "modifDuree").text;
				var shitStory = view.getViewById(this.page, "boringStory").text.toString();

				http.request({
					url: "https://pam-api.duckdns.org/kevfilms/" + this.movie.id.toString(),
					method: "PUT",
					headers: {"Content-Type": "application/json"},
					content: JSON.stringify({
						titre: title,
						duree: napLength,
						scenario: shitStory
					})
				}).then((response) => {
					console.log(JSON.stringify(response));
				}, (e) => {
				});

				this.$navigateTo(movieslistpage);
			},
			onLoaded: function (args) {
				this.page = args.object;


			}
		}
	}


</script>

<style scoped>

    .container {
        padding: 10px;
    }

    .image {
        height: auto;
        width: 100%;
        padding: 5vw;
        margin: 10px;
    }

    .title {
        padding-left: 5vw;
        font-size: 20px;
    }

    .typelv {
        height: 150px;
        width: 50%;
    }

    .movieLabel {
        padding-left: 5vw;
    }

    .text {
        font-size: 15px;
        padding-left: 5vw;
    }

    .button {
        padding: 5vw;
        width: 33%;
    }
</style>

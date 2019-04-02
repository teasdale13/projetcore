<template>
    <Page>
        <ActionBar>
            <label class="actionbarTitle" :text="`FILM/` + this.myFriend.prenom "/>
            <NavigationButton text="Go Back" android.systemIcon="ic_menu_back" @tap="onBackPressed"/>
        </ActionBar>
        <StackLayout>
            <ListView for="item in movieslist" @itemTap="onItemTap">
                <v-template>
                    <StackLayout>
                        <Label :text="item.titre" />
                    </StackLayout>
                </v-template>
            </ListView>

        </StackLayout>
    </Page>
</template>

<script>
    import * as httpWithSSLCertificate from "http"
    import sharedmovieFriend from "./SharedMovies";
    export default {
        props: ["friend"],

        mounted(){
			httpWithSSLCertificate.getJSON("https://pam-api.duckdns.org/kevamis/" + this.friend.id.toString()).then(
                result => {
                    this.myFriend = result;
                    console.log("ALLO!!!! " + JSON.stringify(this.myFriend));

                    /* Va chercher tous les statuts et isole seulement celui dont je veux
                    * car je suis comme ça! Je prends toujours ce qui me conviens et je
                    * laisse le reste de coté! ;) */
					httpWithSSLCertificate.getJSON("https://pam-api.duckdns.org/kevstatuts").then(
                    	result => {
                    		this.allStatus = result;
							for (var x = 0; x < result.length; x++) {
								if (result.description === "Prêté") {
									this.sharedStatus = result[x];
								}
							}
							/* Vérifie si le statut du film est égale au statut "Prêté" */
							for (var xxx = 0; xxx < this.myFriend.kevfilms.length; xxx++){

								/* Procède à la comparaison du Statut "Prêté" avec celui du film dans la
								* liste que l'ami a dans son profil et si les statuts coïcident,
								* il le mets dans la liste de films que l'ami a toujours "chez lui". */
								if (this.myFriend.kevfilms[xxx].kevstatut === this.sharedStatus.id) {
									this.movieslist.push(this.myFriend.kevfilms[xxx]);
                                }
                            }
                        }
                    );
                }
            );

        },
        data(){
            return{
                movieslist: [],
                myFriend: [],
				sharedStatus: [],
				allStatus:[]
            }

        },
        methods: {
            onBackPressed: function () {
                this.$navigateTo(sharedmovieFriend);
            },
			/**
             * En appuyant sur le film dans la ListView ça change le statut de "Prêté" à "Disponible"
             * et ensuite le retire de la liste.
             *
			 * @param index la position dans la liste.
			 */
			onItemTap: function ({index}) {
				httpWithSSLCertificate.request({
					url: "https://pam-api.duckdns.org/kevfilms/" + this.movieslist[index].id.toString(),
					method: "PUT",
					headers: { "Content-Type": "application/json" },
					content: JSON.stringify({
						kevstatut: this.allStatus[0]
					})
				}).then((response) => {
					console.log(JSON.stringify(response));
					this.movieslist.splice(index, 1);

				});
			}
        }
    }
</script>

<style scoped>

</style>
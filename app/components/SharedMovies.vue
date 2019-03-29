<template>
    <Page>
        <ActionBar>
            <label class="actionbarTitle" text="AMI/FILMS"/>
            <NavigationButton text="Go Back" android.systemIcon="ic_menu_back" @tap="onBackPressed"/>
        </ActionBar>
        <StackLayout>
            <Label text="Avec une patate douce!"/>
            <ListView for="item in filteredFriends" @itemTap="onItemTap">
                <v-template>
                    <StackLayout orientation="horizontal" class="listviewcell">
                        <Label :text="item.prenom + ' ' + item.nom" class="movieLabel" textWrap="true"/>
                    </StackLayout>
                </v-template>
            </ListView>
        </StackLayout>

    </Page>
</template>

<script>
    import * as http from "http";
    import home from "./App";
    import detail from "./FriendsMovieList";

    export default {
        mounted(){
            http.getJSON("https://pam-api.duckdns.org/kevamis").then(
                result => {

                    this.friendsList = result;
                    for (var x = 0; x < this.friendsList.length; x++ ){
                        if (this.friendsList[x].kevfilms.length > 0){
                            this.filteredFriends.push(this.friendsList[x]);
                        }
                    }

                },
                error => {
                    console.log(error);
                }
            );
        },
        data() {
            return {
                friendsList: [],
                moviessharedList: [],
                filteredFriends: [],
            }
        },
        methods: {
            onBackPressed: function (event) {
                this.$navigateTo(home);
            },
            onItemTap: function ({index, e}) {
                console.log(JSON.stringify(this.friendsList[index]));
                this.$navigateTo(detail, {
                    props: {
                        friend: this.friendsList[index]
                    }

                });

            }
        }
    }


</script>

<style scoped>

    .movieLabel {
        font-size: 15vw;
        padding: 15px;
    }


</style>

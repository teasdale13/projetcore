<template>
    <Page>
        <ActionBar>
            <label class="actionbarTitle" :text="`FILM/` + this.myFriend.prenom "/>
            <NavigationButton text="Go Back" android.systemIcon="ic_menu_back" @tap="onBackPressed"/>
        </ActionBar>
        <StackLayout>
            <ListView for="item in movieslist">
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
    import * as http from "http"
    import sharedmovieFriend from "./SharedMovies";
    export default {
        props: ["friend"],

        mounted(){
            http.getJSON("http://pam-api.duckdns.org:1337/kevamis/" + this.friend.id.toString()).then(
                result => {
                    this.myFriend = result;
                    console.log("ALLO!!!!" + JSON.stringify(this.myFriend));
                    this.movieslist = this.myFriend.kevfilms;
                    console.log("ALLO!!!!" + JSON.stringify(this.movieslist));
                }
            );

        },
        data(){
            return{
                movieslist: [],
                myFriend: []
            }

        },
        methods: {
            onBackPressed: function (event) {
                this.$navigateTo(sharedmovieFriend);
            },
        }
    }
</script>

<style scoped>

</style>
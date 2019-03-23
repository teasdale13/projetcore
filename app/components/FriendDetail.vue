<template>
    <Page>
        <ActionBar >
            <label class="actionbarTitle" text="NetFilm"/>
            <NavigationButton text="Go Back" android.systemIcon="ic_menu_back" @tap="onBackPressed"/>
        </ActionBar>
        <StackLayout>
            <Label :text="this.myFriend.prenom + ' ' + this.myFriend.nom" />
            <Button text="Supprimer" @tap="executeFriend"/>
        </StackLayout>

    </Page>
</template>

<script>
    import * as http from "http"
    import friendList from "./FriendList"
    export default {
        props: ["friend"],
        mounted(){
            http.getJSON("http://pam-api.duckdns.org:1337/kevamis/" + this.friend.id.toString()).then(
                result => {
                    this.myFriend = result;
                }
            )
        },
        data(){
            return {
                myFriend: []
            }
        },
        methods: {
            onBackPressed: function (event) {
                this.$navigateTo(friendList);
            },
            executeFriend: function () {

                http.request({
                    url: "http://pam-api.duckdns.org:1337/kevamis/" + this.friend.id.toString(),
                    method: "DELETE"
                    }).then((response) => {

                }, (e) => {
                });
                this.$navigateTo(friendList);
            }
        }
    }
</script>

<style scoped>

</style>
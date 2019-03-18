<template>
    <Page>
        <ActionBar>
            <label class="actionbarTitle" text="NetFilm"/>
            <NavigationButton text="Go Back" android.systemIcon="ic_menu_back" @tap="onBackPressed"/>
        </ActionBar>
        <StackLayout>
            <Label text="Avec une patate douce!"/>
            <ListView for="item in sharedlist" @itemTap="onItemTap">
                <v-template>
                    <StackLayout orientation="vertical">
                        <Label :text="item.movie" class="movieLabel" textWrap="true"/>
                    </StackLayout>
                </v-template>
            </ListView>
        </StackLayout>

    </Page>
</template>

<script>
    import home from "./App";

    export default {
        data() {


            return {

                randomQuestion: '',
                randomResponse: '',
                sharedlist: [{movie: "Par derrière, tu entreras."}, {movie: "Je la prends toute! (la pointe de tarte)"}, {movie: "J'ai l'Doua! Chu mécanicien!"},
                    {movie: "Il était une fois.... une fin!"}, {movie: "À cause de ma bedaine, je ne la vois plus. (la balance lorsque je me pèse!)"},
                    {movie: "Lorsque je la sort, tout le monde est émerveillé par ma voiture antique."}],

            }
        },
        methods: {
            onBackPressed: function (event) {
                this.$navigateTo(home);
            },
            onItemTap: function (args) {
                console.log("onItemTap");
                alert("Vous avez cliqué sur un film qui est prêté");

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

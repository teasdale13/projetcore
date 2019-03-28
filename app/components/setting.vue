<template>
    <Page>
        <ActionBar>
            <label class="actionbarTitle" text="SETTINGS"/>
            <NavigationButton text="Go Back" android.systemIcon="ic_menu_back" @tap="onBackPressed"/>
        </ActionBar>
        <Label text="ALLLOOOOOOO" />
    </Page>
</template>

<script>
    import home from "./App"
    export default {
        data(){
            return{

            }
        },
        methods:{
            onBackPressed: function() {
                this.$navigateTo(home);
            },
        }

    }
</script>

<style scoped>

</style>
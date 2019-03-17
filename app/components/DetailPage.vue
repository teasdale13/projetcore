<template>
    <Page>
        <ActionBar>
            <label class="actionbarTitle" text="NetFilm"/>
             <NavigationButton text="Go Back" android.systemIcon="ic_menu_back" @tap="onBackPressed"/>
        </ActionBar>
        <StackLayout>
          <Label class="message" text="Je suis la page de details"/>
          
        </StackLayout>
            
    </Page>
</template>

<script >
import movieslistpage from "./MoviesListPage";
  export default {
    data() {
      return {
        
      }
    },
    methods: {
    onBackPressed: function(event){
        this.$navigateTo(movieslistpage);
    }
  }
  }
  

</script>

<style scoped>
    ActionBar {
        background-color: #53ba82;
        color: #ffffff;
        text-align: center;
    }
.actionbarTitle{
text-align: center;
font-size: 20px;
font-weight: bold;
font-family: sans-serif;
}
    .message {
        vertical-align: center;
        text-align: center;
        font-size: 20;
        color: #333333;
    }
</style>

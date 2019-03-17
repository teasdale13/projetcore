<template>
    <Page>
        <ActionBar>
            <label class="actionbarTitle" text="NetFilm"/>
             <NavigationButton text="Go Back" android.systemIcon="ic_menu_back" @tap="onBackPressed"/>
        </ActionBar>
        <StackLayout>
          <Label class="message" text="Formulaire"/>
          <Button text="Take a picture" @tap="takePhoto"/>
          <Image :src="img" width="200" height="200"/>
        </StackLayout>
            
    </Page>
</template>

<script >
import { Image } from "tns-core-modules/ui/image";
import * as camera from "nativescript-camera";
import movieslistpage from "./MoviesListPage";

  export default {
    data() {
      return {
        img:''
      }
    },
    methods: {
    onBackPressed: function(event){
        this.$navigateTo(movieslistpage);
    },
    takePhoto(){
        camera.requestPermissions().then(() =>{
            camera.takePicture({ width: 300, height: 300, keepAspectRatio: true, saveToGallery:true })
            .then((imageAsset) => {
                var image = new Image();
                image.src = imageAsset;
                this.img = imageAsset;
            })
            .catch(e => {
					console.log('error:', e);
                });
                })
			.catch(e => {
				console.log('Error requesting permission');
			});
        
    }
  }
  }
  

</script>

<style scoped>
    ActionBar {
        text-align: center;
        background-color: #53ba82;
        color: #ffffff;
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

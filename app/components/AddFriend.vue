<template>
    <Page class="dialog">
        <FlexboxLayout class="stack" flexDirection="column">
            <Label text="Entrez le prénom:" textWrap="true" width="100%"/>
            <TextField text="Allo" hint="Prénom" id="firstname" class="editText" width="100%"/>
            <Label text="Entrez le nom:" textWrap="true" width="100%"/>
            <TextField hint="Nom" class="editText" width="100%"/>
            <Button text="Confirmer" @tap="$modal.close()"/>

        </FlexboxLayout>
    </Page>
</template>

<script>
    var view = require("ui/core/view");
    export default {
        data(){
            name: "AddFriend"
        },
        methods: {


        }


    }
</script>

<style scoped>
    .dialog{
    /* min-width: 500px;*/
    min-height: 600px;
}
.stack{
    width: 100%;
}

</style>
<template>
<div>
    <v-img :src="this.imgRef" height="200px">
    </v-img>
</div>
</template>
<script>
import axios from 'axios'
export default {
    name: "giphy",
    data: function(){
        return{
            imgRef: String
        }

    },
    props: {
        desc: String
    },
    created: function(){
        var giphySearch = 'https://api.giphy.com/v1/gifs/search?q='+this.desc+'&api_key=LLtGzlJAN6176Sd5eDEJ6rspOnq1maJo&limit=10'
       
       axios.get(giphySearch).then((response) => {
           this.imgRef = response.data.data[0].images.original.url
       })
    }
}
</script>

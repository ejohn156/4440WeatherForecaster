<template>
<div>
    <v-img :src="this.props.desc" height="200px">
    </v-img>
</div>
</template>
<script>
//import axios from 'axios'
export default {
    name: "giphy",
    
    props: {
        desc: String
    },
    computed: {
        loaded(){
            return this.desc
        }
    },
}
</script>

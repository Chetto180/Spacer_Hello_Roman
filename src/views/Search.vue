<template>
  <div class="wrapper">
      <label for="search">Search</label>
      <input type="text"
       class="search" 
       name="search" 
       v-model="searchValue"
       @input="handleInput">
       <ul>
            <li v-for="(item,index) in results" :key="index"><p>{{item.data[0].description}}</p></li>
       </ul>
  </div>
</template>

<script>
import axios from 'axios';
import debounce from 'lodash.debounce';

const API = 'https://images-api.nasa.gov';

export default {
    name: 'Search',   
    data(){
        return{
            searchValue: '',
            results: []
        }
    },
    methods:{
        handleInput: debounce(function(){
            axios.get(`${API}/search?q=${this.searchValue}&media_type=image`)
            .then(response=>{
                this.results = response.data.collection.items;
                console.log(this.results)
            })
            .catch(error=>console.log(error));
        },500)
    }
}
</script>

<style lang="scss" scoped>
    .wrapper{
        width: 100%;
        display: flex;
        align-items: center;
        flex-direction: column;
        margin: 0;
        padding: 30px;
    }
    .search{
        display: flex;
        flex-direction: column;
        width: 300px;
    }
    label{
        font-family: 'Franklin Gothic Medium', 'Arial Narrow', Arial, sans-serif;
    }
    input{
        height: 60px;
        border: 0;
        border-bottom: 1px solid black;
    }
</style>

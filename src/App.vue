<template>
 
  <div class="container-fluid pb-3" style="height: 100vh;">
    <Header class="fixed-top"></Header>
    <div
      class="row  justify-content-center mt-5"
      style="background: #fff; padding: 3rem 0rem; 
      border-radius: 20px;
       box-shadow: rgba(50, 50, 93, 0.25) 0px 30px 60px -12px inset, rgba(0, 0, 0, 0.3) 0px 18px 36px -18px inset;"
      >
      <!-- input field start -->
      <div class="row justify-content-center mb-4">
        <div class="col-sm-12 col-md-6">
      
          <div class="input-group mb-3">
            <input
              type="text"
              class="form-control"
              placeholder="Search Movies"
              aria-label="Recipient's username"
              aria-describedby="button-addon2"
              @keypress.enter="fetchApi()"
              v-model="mod"/>
            <button
              class="btn btn-outline-secondary"
              type="button"
              id="button-addon2"
              @click="fetchApi()">Search
            </button>
          </div>
        </div>
      </div>
      <!-- input field end -->

      <div class="row justify-content-center" v-show="imgisTrue">
        <div class="col-sm-12 col-md-8 text-center">
          <img src="./assets/loading.gif" alt="" style="width:200px; height:100px; border-radius: 50%;">
        </div>
      </div>


      <div class="col-sm-12 col-md-8 d-flex justify-content-center mt-4" v-for="d in des" :key="d">
        
        <!-- card start -->
        <div class="card" style="width: 30rem;">
          <img :src="`https://image.tmdb.org/t/p/w500/${d.poster_path}`" class="card-img-top img-fluid" alt="..." />
          <div class="card-body">
            <h5 class="card-title">{{d.original_title}}</h5>
            <p class="card-text">
              {{d.overview}}
            </p>
            <a href="#" class="btn btn-primary">Go somewhere</a>
          </div>
        </div>
        <!-- card end -->
      </div>
    </div>
    <Footer class="fixed-bottom"></Footer>
  </div>
  
</template>

<script>
import axios from "axios";
import Header from "./components/Header.vue"
import Footer from "./components/Footer.vue";
export default {
  data() {
    return {
      des: [],
      mod: "",
      imgisTrue:false
    };
  },
  components:{
    Header,
    Footer
},
  methods: {
    fetchApi() {
      // console.log(this.mod);
      var query=this.mod;
      this.imgisTrue=true;
      this.mod = "";
      axios
        .get(
          `https://api.themoviedb.org/3/search/movie?api_key=97af7fddb6c5575fdb0ccef104d2dea7&query=${query}`
          
        )
        .then((data) => {
          var arr = [];
          arr = data.data.results;
          if(arr <=0){
            this.imgisTrue=false;
            return alert("record not found");
            
          }
          this.des = arr;
          console.log(data.data.results);
          this.imgisTrue=false;
          
        });
    },
    
  },
};
</script>

<style>
body {
  background: #e2e2e2;
  width: 100%;
  height: 100vh;
 
}
img{
    max-height: 250px;
    object-fit: cover;
  }
  
  .form-control:focus {
    border-color: none;
    box-shadow: none;
    } 
    
</style>
<template>
  <div>
    <div class="input-group search search-box col-md-4 col-xs-6">
      <div class="form-group has-feedback">
        <input type="search" class="form-control" style=""
          v-model="searchTerm" placeholder="Search" 
          v-on:keyup.enter="seacrhImplement">
        <span class="form-control-clear glyphicon glyphicon-remove form-control-feedback" 
          @click="clearSearch" v-if="searchTerm">
        </span>
      </div>
      <span class="input-group-btn">
        <button class="btn btn-danger" type="button" @click="seacrhImplement">
            <span class=" glyphicon glyphicon-search"></span>
        </button>
      </span>              
    </div>
    </div>
</template>

<script>
  export default {
      data () {
          return {
             searchTerm: this.searchExpression
          }
      },
      props: ['searchExpression'],
      methods: {
          seacrhImplement () {
            if(this.searchTerm){
              this.$emit('search', this.searchTerm);
            }
          },
          clearSearch() {
            this.searchTerm = "";
          }
      }
    }
</script>

<style>
  .search{
    position: absolute;
    text-align: center;
  }

  .search-box{
    margin-left: 30%;
    margin-top: 40px;
  }

  .search-box input{
    border-radius: 25px;
  }

  .form-control-clear {
    z-index: 10;
    pointer-events: auto;
    cursor: pointer;
  }

  .clear-form{
    background: transparent;
  }

  .search-query:focus .clear-form{
    display: block;
  }
</style>

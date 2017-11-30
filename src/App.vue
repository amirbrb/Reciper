<template>
	<div id="app">
		<div class="thinker-wrapper" v-if="isLoading">
			<div class="thinker">
				<img src="static/img/icons/loading.gif">
			</div>
		</div>
		<Search v-on:search="search" :searchExpression="searchSettings.searchTerm"/>
		<div class="results-container">
			<Result v-for="result in storageElement.state.results" 
				v-bind:recipe="result.recipe"
				v-bind:isBookmarked="isBookmarked(result.recipe.uri)"
				v-on:selected="selectRecipe"
				v-on:bookmarked="bookmark">
			</Result>
			<div class="load-more reciper-button big red">
				<span class="glyphicon glyphicon-plus-sign" v-if="searchSettings.hasMoreResults && storageElement.state.results && storageElement.state.results.length > 0" @click="loadMore"></span>
			</div>
		</div>
		<RecipeViewer
			v-if="isSelected"
			v-bind:selectedRecipe="selectedRecipe"
			v-on:closed="closedViewer"/>
  	</div>
</template>

<script>
	import Search from './components/Search';
	import Result from './components/Result';
	import RecipeViewer from './components/RecipeViewer';
	import axios from 'axios';

	export default{
		name: 'app',
		created () {
			var self = this;
			var storage = window.localStorage;
			var storageElement = storage.recatcherElement;
			if(storageElement){
				self.storageElement = JSON.parse(storageElement);
				self.searchSettings.searchTerm = self.storageElement.state.searchExpression;
				self.searchSettings.from = self.storageElement.state.results ? self.storageElement.state.results.length : 0;
			}
		},
	  	components: {
	    	Search,
	    	Result,
	    	RecipeViewer
	  	},
	  	data () {
	    	return {
				isSelected: false,
				isLoading: false,
				searchSettings: {
					searchTerm: '',	
					from: 0,
					numOfResultsPerRequest: 12,
					hasMoreResults: true
				},
				selectedRecipe: {

				},
				storageElement: {
					state: {
						results: [],
						searchExpression: ''
					},
					userData: {
						bookmarks: {

						}	
					}
				}
	    	}
	  	},
	  	methods: {
	    	search(searchExpression) {
	    		var self = this;
	    		self.isLoading = true;
	    		self.storageElement.state.results = [];	
	    		self.storageElement.state.searchExpression = searchExpression;

    			self.searchSettings.searchTerm = searchExpression;
				var url = 'https://api.edamam.com/search?from=0&to=' + self.searchSettings.numOfResultsPerRequest + '&q=' + searchExpression;
				axios.get(url)    		
				    .then(response => {
					    self.storageElement.state.results = response.data.hits;  
					    self.saveStorage(self.storageElement);
					    self.isLoading = false;
					    self.searchSettings.hasMoreResults = response.data.more;
				    })
			    .catch(e => {
		      		self.isLoading = false;
			    });
	    	},
			saveStorage(storageElement){
				window.localStorage.setItem('recatcherElement', JSON.stringify(storageElement));
			},
			loadMore() {
				var self = this;
				self.isLoading = true;
				var url = 'https://api.edamam.com/search?from=' + (self.searchSettings.from + 1 + self.searchSettings.numOfResultsPerRequest) + 
					'&to=' + (self.searchSettings.from + 1 + self.searchSettings.numOfResultsPerRequest + self.searchSettings.numOfResultsPerRequest) + 
					'&q=' + self.storageElement.state.searchExpression;
				axios.get(url)    		
				    .then(response => {
					    self.storageElement.state.results = self.storageElement.state.results.concat(response.data.hits);  
					    self.saveStorage(self.storageElement);
					    self.isLoading = false;
					    self.searchSettings.from += response.data.hits.length;
					    self.searchSettings.hasMoreResults = response.data.more;
				    })
			    .catch(e => {
		      		self.isLoading = false;
			    });	
			},
	    	selectRecipe(recipe){
	    		var self = this;
	    		self.isLoading = true;
	    		var url = 'https://api.edamam.com/search?r=' + recipe.uri.replace('#', '%23');
				axios.get(url)    		
				    .then(response => {
				    	var data = response.data;
				    	if(data.length > 0){
							var firstResult = data[0];
				    		self.selectedRecipe = firstResult;
				    	}

					    self.isLoading = false;
					    self.isSelected = true;
				    });
	    	},
	    	closedViewer(){
	    		this.isSelected = false;
	    	},
	    	isBookmarked(uri){
	    		var self = this;
	    		if(self.storageElement.userData.bookmarks){
	    			var recipe = self.storageElement.userData.bookmarks[uri];
	    			var isBookmarked = typeof recipe !== 'undefined';
	    			return isBookmarked;
	    		}

	    		return false;
	    	},
	    	bookmark(isRemove, recipe){
	    		var self = this;
    			if(isRemove){
    				debugger;
    				delete self.storageElement.userData.bookmarks[recipe.uri];
    			}
    			else{
    				debugger;
    				self.storageElement.userData.bookmarks[recipe.uri] = recipe
    			}

    			self.saveStorage(self.storageElement);
	    	}
	  	}
	}
</script>

<style>
	.results-container{
		position:absolute;
		top: 100px;
		left: 6px;
		text-align: center;
	}

	.thinker-wrapper{
	    z-index: 100000;
	    display: flex;
	    display: -webkit-box;
	    display: -moz-box;
	    display: -webkit-flexbox;
	    display: -ms-flexbox;
	    display: -webkit-flex;
	    -ms-flex-direction: column;
	    -o-flex-direction: column;
	    -webkit-flex-direction: column;
	    flex-direction: column;
	    -o-justify-content: center;
	    -webkit-justify-content: center;
	    justify-content: center;
	    -ms-flex-pack: start;
	    -ms-flex-line-pack: center;
	    -moz-align-content: center;
	    -o-align-content: center;
	    -webkit-align-content: center;
	    align-content: center;
	    position:fixed;
	    padding:0;
	    margin:0;
	    top:0;
	    left:0;
	    width: 100%;
	    height: 100%;
    	background:rgba(255,255,255,0.6);
	}

	.thinker{
		margin: auto;
	}

	.thinker img{
		width:300px;
		height:350px;
	}

	.load-more{
		position: relative;
	    bottom: 0;
	    left: 0;
	    height: 0px;
	}

	.reciper-button:hover{
		text-decoration: none;
	}

	.reciper-button span{
		cursor: pointer;
	}

	.reciper-button.big{
		font-size: 50px;
	}

	.reciper-button.medium{
		font-size: 40px;
	}

	.reciper-button.small{
		font-size: 30px;
	}

	.reciper-button.red, .reciper-button.red span{
		color: red;
	}

	.reciper-button.red:hover, .reciper-button.red span:hover{
		color: darkred;	
	}
</style>

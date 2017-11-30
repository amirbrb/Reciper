<template>
	<div id="recipe-viewer" class="selected-result-container">  
		<a class="reciper-button close-result" @click="closeViewer"><span class="glyphicon glyphicon-remove-circle"></span></a>
		<div class="selected-result">
			<div class="header">
				<h4>{{this.selectedRecipe.label}}</h4>
			</div>
			<div class="body">
				<div>
					<img class="img img-circle"  v-bind:src="selectedRecipe.image"/>
				</div>
				<div class="ingredients">
					<label>INGREDIENTS:</label>
					<div class="data">
						<ul>
							<li v-for="ingredient in selectedRecipe.ingredientLines">{{ingredient}}</li>
						</ul>
					</div>
				</div>
				<div class="navigator">
					<a class="reciper-button small red" href="#" @click="openInApp">
						<span class="glyphicon glyphicon-circle-arrow-right"></span>CHECK IT
					</a>	
				</div>
			</div>
		</div>
	</div>
</template>

<script>
	export default{
	  	data () {
	    	return {
				
	    	}
	  	},
	  	props: ['selectedRecipe'],
	  	methods: {
	    	closeViewer() {
	    		this.$emit('closed');
	    	},
	    	openInApp(){
	    		var self = this;
	    		var ua = navigator.userAgent.toLowerCase();
	    		if(ua.indexOf("android") > -1){
    				window.open(self.selectedRecipe.url, '_self ', 'location=yes');
	    		}
	    		else if (ua.indexOf("mac os x") > -1){
		    		window.open(self.selectedRecipe.url, '_system');
	    		}
	    		else{
	    			window.open(self.selectedRecipe.url, 'self');
	    		}
	    	}
	  	}
	}
</script>

<style scoped>
	.body img{
		height: 250px;
	}

	.navigator{
		font-size: 20px;
		margin-top: 20px;
	}

	.selected-result-container{
	    background-color: #fff;
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
	    text-align:center;
	}

	.close-result{
		position: absolute;
		left:20px;
		top: 20px;
	}

	.close-result span{
		font-size: 20px;
		color: gray;	
	}

	.glyphicon {
	    display:block;
	}

	.selected-result{
		margin: auto;
		margin-top: 40px;
	}

	.header{
		width: 60%;
		margin-left:70px;
	}

	.body{
		width: 100%;
	}	

	.ingredients{
		margin-top: 5px;
	}

	.data {
		height: 160px;
		overflow: auto;
	}

	.body li{
		list-style: square;
		text-align: left;
		font-size: 18px;
	}
</style>

<template>
	<div class="result col-md-2 col-xs-12">
		<div class="header">
			<div class="actions">
				<i v-bind:class="['glyphicon', bookmarked ? 'glyphicon-star' : 'glyphicon-star-empty']" @click="bookmark"></i>
			</div>
		</div>
		<div class="body" @click="selectRecipe">
			<label>{{recipe.label}}</label>
			<img v-bind:src="recipe.image"/>
		</div>
		<ShareThis :recipe="recipe"></ShareThis>
  	</div>
</template>

<script>
	import ShareThis from './ShareThis';
	export default {
    	data () {
    		return {
    			isSharing: true,
    			bookmarked: false
    		}
	    },
	    created (){
	    	this.bookmarked = this.isBookmarked;
	    },
	    props: ['recipe', 'isBookmarked'],
	    components: {
	    	'ShareThis': ShareThis
	    },
	    methods: {
      		selectRecipe (){
      			this.$emit('selected', this.recipe);
      		},
      		bookmark (){
      			var self = this;
      			self.$emit('bookmarked', self.isBookmarked, self.recipe);
      			self.bookmarked = !self.bookmarked;
      		},
      		share(){
      			this.isSharing = true
      		}
	    }
  	}
</script>

<style scoped>
	.result{
		float: left;
		text-align: center;
		cursor: pointer;
		border-radius: 5px;
		border: grey solid 0.4pt;
		margin-top: 1px;
		height: 320px;
	}

	.header{
		margin-top: 10px;
		max-height:80px;
	}

	.actions{
		position: absolute;
		top:5px;
		right:10px;
		font-size: 28px;
	}

	.body{
		padding-left: 5px;
		padding-right: 5px;
	}

	.body label{
		height: 70px;
		width:80%;
	}

	.body img{
		height: 200px;
		width: 200px;
		border-radius: 100px;
	}
</style>

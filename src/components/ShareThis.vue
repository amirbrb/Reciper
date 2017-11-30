<template>
	<div class="share-this">
		<a class="share" @click="shareWhatsapp">
			<div class="fa fa-whatsapp"></div>
		</a>
		<a :href="mailFormat" class="share">
			<div class="fa fa-envelope"></div>
		</a>
  	</div>
</template>

<script>
	export default {
    	data () {
    		return {
    			mailFormat: "mailto:?subject=I thought you might like this one&body=i saw this recipe and thought you might like it. what do you say? <a href='" + this.recipe.url + "'>" + this.recipe.label + "<a/>",
    			whatsappFormat: "whatsapp://send?text=I thought you might like this one: " + this.recipe.url
    		}
	    },
	    props: ['recipe'],
	    methods: {
	    	shareWhatsapp() {
	    		window.plugins.socialsharing.shareViaWhatsApp('Message via WhatsApp', null /* img */, null /* url */, function() { console.log('share ok') }, function(errormsg) { alert(errormsg) });
	    	}
	    }
  	}
</script>

<style scoped>
	.share-this{
		position: absolute;
		left: 10px;
		bottom: 2px;
	}

	.share{
		font-size: 28px;
		color: black;
		margin-left: 10px;
	}
</style>

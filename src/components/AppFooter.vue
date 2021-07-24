<template>
	<footer>
		<div>
			Made with <span class="color-red">❤</span> by 
			<a 
				href="https://mubasharabbas.me"
				target="_blank"
			>Mubashar Abbas</a>
		</div>
	</footer>
</template>

<script>
export default {

}
</script>

<style scoped>
	.color-red {
		color: red;
	}
	footer {
		background: black;
		color: white;
		padding: 20px;
		height: 100px;
		border-top: 10px solid rgb(37, 30, 30);
		display: flex;
		align-items: center;
		justify-content: center;
	}
	footer a {
		text-decoration: none;
		color: skyblue;
	}
</style>
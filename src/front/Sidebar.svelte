<script>
	export let open = false
</script>

<aside class="absolute w-full h-full bg-gray-200 border-r-2 shadow-lg" class:open>
	<nav class="p-12 text-xl">
		<a class="block" href="/#/info">Sobre nosotros</a>
		<a class="block" href="/public/contact.html">Contacto</a>
	</nav>
</aside>
<style>
	aside {
		left: -100%;
		transition: left 0.3s ease-in-out
	}
	
	.open {
		left: 0
	}
</style>
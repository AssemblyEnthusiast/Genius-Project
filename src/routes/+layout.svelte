<script lang="ts">
    import '../app.css';

    let { children } = $props();

</script>

<div class="flex flex-col w-full bg-gray-100">
    <div class="w-full bg-black h-24 flex items-center px-8 justify-between">
        <a href="/" class="text-white text-4xl">Genius Project</a>
        <div class="flex justify-center mt-4 text-white">
            <a href="/pag1" class="btn btn-primary mx-2">Page 1</a>
            <a href="/pag2" class="btn btn-primary mx-2">Page 2</a>
            <a href="/pag3" class="btn btn-primary mx-2">Page 3</a>
            <a href="/pag4" class="btn btn-primary mx-2">Page 4</a>
            <a href="/pag5" class="btn btn-primary mx-2">Page 5</a>
        </div>
    </div>
    <div class="min-h-svh w-full">
        {@render children()}
    </div>
</div>

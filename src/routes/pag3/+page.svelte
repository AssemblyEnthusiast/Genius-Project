<script>
    let title = "Welcome to Page 3";
    let description = "This is a simple Svelte page for your website.";
</script>

<main>
    <h1>{title}</h1>
    <p>{description}</p>
    <nav>
        <a href="/">Home</a>
        <a href="/pag2">Page 2</a>
        <a href="/pag3">Page 3</a>
    </nav>
</main>

<style>
    main {
        max-width: 600px;
        margin: 2rem auto;
        padding: 2rem;
        background: #f9f9f9;
        border-radius: 8px;
        box-shadow: 0 2px 8px rgba(0,0,0,0.05);
        text-align: center;
    }
    nav a {
        margin: 0 1rem;
        color: #0070f3;
        text-decoration: none;
        font-weight: bold;
    }
    nav a:hover {
        text-decoration: underline;
    }
</style>
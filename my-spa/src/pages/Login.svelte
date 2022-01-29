<script>
    const title = "Login";
    $: {
        document.title = title;
    }
</script>

<main>
    <h1>{title}</h1>
    <p>
        This is a sample SPA app that can do sign-in via the OIDC Authorization
        Code with PKCE Flow.
    </p>
    <p>
        <input type="submit" value="Log in with OIDC" />
    </p>
</main>

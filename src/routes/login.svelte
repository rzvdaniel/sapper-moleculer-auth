<svelte:head>
	<title>Sign in</title>
</svelte:head>

<h1>Sign In</h1>
<p><a href="/register">Need an account?</a></p>
<p><a href="/reset">Forgotten?</a></p>

<Error {error} />

<form on:submit|preventDefault='{submit}'>
    <input type="email" placeholder="Email" bind:value={username}>
    <input type="password" placeholder="Password" bind:value={password}>
    <button type="submit" disabled='{!username || !password}'>Sign in</button>
</form>

<script>

	import { goto, stores } from '@sapper/app';
    import { auth } from '../library/stores.js';
    import Error from '../components/Error.svelte';

    const { session } = stores();

    let username = 'rzvdaniel@gmail.com';
    let password = '$Daniel1';
    let error = false;

    async function submit(event) {

        const response = await auth.login({ username, password });

        if (response.email === username) {

            $session.user = response;

            goto('/');
        }
        else {
            error = response.message;
        }
    }
</script>
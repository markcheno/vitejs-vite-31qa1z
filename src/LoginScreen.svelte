<script lang="ts">
  import { pocketbase, errorMessage } from "./lib/pocketbase";

  let email: string;
  let password: string;
  let loading: boolean = false;
  let register: boolean = false;
  let message: string = "";

  const handleLogin = async () => {
    loading = true;

    if (register) {
        try {
          await pocketbase.collection("users").create({
            email,
            password,
            passwordConfirm: password,
          });

          try {
            await pocketbase.collection("users").requestVerification(email);
            message = "Check your email for the login link";
          } catch (error) {
            message = errorMessage(error);
          }
        } catch (error) {
          message = errorMessage(error);
        }
        register = false;
    } else {
      try {
          await pocketbase.collection("users").authWithPassword(email, password);
      } catch (error) {
          message = errorMessage(error);
      }
    }

    loading = false;
  };
</script>

<form on:submit|preventDefault={handleLogin}>
      <input 
        placeholder="Email address"
        id="email-address"
        name="email"
        type="email"
        autocomplete="email"
        size="40"
        required
        bind:value={email}
      >
      <input 
        type="password" 
        placeholder="Password"
        id="password"
        name="password"
        autocomplete="current-password"
        size="40"
        required
        bind:value={password}
      >
      <button 
        type="submit">
        {loading ? "Signing in...." : "Sign in"}
      </button>
      <button 
        type="submit" 
        disabled={loading} 
        on:click={() => (register = true)}
      >
        {loading && register ? "Registering...." : "Register"}
      </button>
  {#if message}
    <p>{message}</p>
  {/if}
</form>

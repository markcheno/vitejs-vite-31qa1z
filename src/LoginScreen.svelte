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

<section>
    <form on:submit|preventDefault={handleLogin}>
      <header>
        <h2>Sign in to your account</h2>
      </header>
      <label for="email-address">Email address</label>
      <input
        id="email-address"
        name="email"
        type="email"
        autocomplete="email"
        size="40"
        required
        placeholder="Email address"
        bind:value={email}
      />
      <label for="password">Password</label>
      <input
        id="password"
        name="password"
        type="password"
        autocomplete="current-password"
        size="40"
        required
        placeholder="Password"
        bind:value={password}
      />

      {#if message}
      <p>{message}</p>
      {/if}

      <button 
        type="submit" 
        disabled={loading}
      >
        {loading ? "Signing in...." : "Sign in"}
      </button>

      <button 
        type="submit" 
        disabled={loading} 
        on:click={() => (register = true)}
      >
        {loading && register ? "Registering...." : "Register"}
      </button>
    </form>
 </section>
<script lang="ts">
  import Header from "./Header.svelte";
  import LoginScreen from "./LoginScreen.svelte";
  import { currentUser } from "./lib/pocketbase";
  import UserScreen from "./UserScreen.svelte";
</script>

<Header />
<main>
{#if !$currentUser}
  <LoginScreen />
{:else}
  <UserScreen />
{/if}
</main>
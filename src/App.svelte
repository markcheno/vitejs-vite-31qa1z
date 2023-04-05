<script lang="ts">
  import LoginScreen from "./LoginScreen.svelte";
  import { currentUser } from "./lib/pocketbase";
  import UserScreen from "./UserScreen.svelte";
</script>

<main>
{#if !$currentUser}
  <LoginScreen />
{:else}
  <UserScreen />
{/if}
</main>
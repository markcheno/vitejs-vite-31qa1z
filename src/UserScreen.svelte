<script lang="ts">
  import { pocketbase, currentUser, errorMessage } from "./lib/pocketbase";

  async function handleSignOut() {
    try {
      console.log("signing out")
      pocketbase.authStore.clear();
    } catch (error) {
      errorMessage(error);
    }
  }
</script>


<div>
  <header>{$currentUser.email}</header>
  <button 
    on:click={handleSignOut}>
    Sign Out
  </button>
</div>

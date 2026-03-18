<script>
  import Mercados from './lib/components/Mercados.svelte';
  import Counter from './lib/components/Counter.svelte';
  import BottomNav from './lib/components/BottomNav.svelte';
  import { activeTab } from './lib/stores/store.js';

  $: tab = $activeTab;
</script>

<div class="app-container">
  <main class="app-content">
    {#if tab === 'home'}
      <div class="tab-content animate__animated animate__fadeIn">
        <Counter />
      </div>
    {:else if tab === 'market'}
      <div class="tab-content animate__animated animate__fadeIn">
        <Mercados />
      </div>
    {/if}
  </main>

  <footer class="app-footer-text text-center text-muted py-3 pb-5 mb-5">
    <p>&copy; {new Date().getFullYear()} PoupaPoupa. Por Renatinho.</p>
  </footer>

  <BottomNav />
</div>

<style>
  @import 'https://cdnjs.cloudflare.com/ajax/libs/animate.css/4.1.1/animate.min.css';

  :global(body) {
    font-family: 'Segoe UI', system-ui, -apple-system, sans-serif;
    background-color: #f4f6f9;
    color: #2c3e50;
    margin: 0;
    padding: 0;
    overflow-x: hidden;
  }

  .app-container {
    display: flex;
    flex-direction: column;
    min-height: 100vh;
    padding-top: 0; /* No space needed for header anymore */
    padding-bottom: 70px; /* Space for bottom nav */
  }

  .app-content {
    flex: 1;
    padding: 0 15px 15px 15px; /* Removed top padding */
    max-width: 600px;
    width: 100%;
    margin: 0 auto;
  }

  .tab-content {
    width: 100%;
  }

  .app-footer-text {
    font-size: 0.8rem;
    opacity: 0.6;
  }

  /* Override generic cards for a more modern mobile look */
  :global(.card) {
    border: none !important;
    border-radius: 16px !important;
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.05) !important;
    margin-bottom: 20px !important;
  }

  /* Make sure inputs look good on mobile */
  :global(.form-control, .form-select) {
    border-radius: 12px !important;
    padding: 12px 16px !important;
    border: 1px solid #e0e0e0 !important;
  }

  :global(.form-control:focus, .form-select:focus) {
    box-shadow: 0 0 0 3px rgba(0, 123, 255, 0.1) !important;
    border-color: #007bff !important;
  }

  :global(.btn) {
    border-radius: 12px !important;
    padding: 10px 20px !important;
    font-weight: 600 !important;
  }
</style>
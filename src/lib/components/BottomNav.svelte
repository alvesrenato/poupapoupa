<script>
  import { activeTab } from '../stores/store.js';

  function setTab(tab) {
    activeTab.set(tab);
  }
</script>

<nav class="bottom-nav">
  <button 
    class="nav-item" 
    class:active={$activeTab === 'home'} 
    on:click={() => setTab('home')}
  >
    <span class="icon">💰</span>
    <span class="label">Gastos</span>
  </button>
  
  <button 
    class="nav-item" 
    class:active={$activeTab === 'market'} 
    on:click={() => setTab('market')}
  >
    <span class="icon">🛒</span>
    <span class="label">Local</span>
  </button>
</nav>

<style>
  .bottom-nav {
    position: fixed;
    bottom: 0;
    left: 0;
    right: 0;
    height: 65px;
    background: #ffffff;
    display: flex;
    justify-content: space-around;
    align-items: center;
    box-shadow: 0 -2px 10px rgba(0, 0, 0, 0.1);
    z-index: 1000;
    padding-bottom: env(safe-area-inset-bottom);
  }

  .nav-item {
    flex: 1;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    background: none;
    border: none;
    padding: 8px 0;
    color: #6c757d;
    transition: color 0.3s, transform 0.2s;
  }

  .nav-item.active {
    color: #007bff;
  }

  .icon {
    font-size: 1.5rem;
    margin-bottom: 2px;
  }

  .label {
    font-size: 0.75rem;
    font-weight: 500;
  }

  .nav-item:active {
    transform: scale(0.95);
  }
</style>

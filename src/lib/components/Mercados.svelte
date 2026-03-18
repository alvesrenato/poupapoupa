<script>
  import { onMount } from 'svelte';
  import { nomeMercado } from '../stores/store.js';

  let mercadoSelecionado = '';
  const mercadosComuns = ['Pingo Doce', 'Continente', 'Lidl', 'Auchan', 'Mercadona'];
  const todasAsOpcoesDeMercado = [...mercadosComuns, 'Outro'];

  let outroMercadoDigitado = '';

  onMount(() => {
    mercadoSelecionado = mercadosComuns[0];
    nomeMercado.set(mercadoSelecionado);
  });

  $: {
    if (mercadoSelecionado === 'Outro') {
      nomeMercado.set(outroMercadoDigitado.trim());
    } else {
      nomeMercado.set(mercadoSelecionado);
      outroMercadoDigitado = '';
    }
  }
</script>

<div class="market-container">
  <div class="mb-4 text-center">
    <h2 class="h4 fw-bold">Onde vai comprar? 🛒</h2>
    <p class="text-muted small">Selecione o estabelecimento para organizar seus gastos.</p>
  </div>

  <div class="selection-grid">
    {#each todasAsOpcoesDeMercado as mercadoOpcao}
      <label class="chip-card" class:selected={mercadoSelecionado === mercadoOpcao}>
        <input
          type="radio"
          name="mercado-opcao"
          value={mercadoOpcao}
          bind:group={mercadoSelecionado}
        />
        <div class="chip-content">
          <span class="chip-icon">
            {#if mercadoOpcao === 'Outro'} 📍 {:else} 🏪 {/if}
          </span>
          <span class="chip-label">{mercadoOpcao}</span>
        </div>
      </label>
    {/each}
  </div>

  {#if mercadoSelecionado === 'Outro'}
    <div class="mt-4 animate__animated animate__fadeIn">
      <div class="card p-3 border-0 bg-white">
        <label for="outroMercadoInput" class="form-label fw-bold">Nome do local</label>
        <input
          id="outroMercadoInput"
          type="text"
          class="form-control"
          placeholder="Ex: Farmácia, Papelaria..."
          bind:value={outroMercadoDigitado}
        />
      </div>
    </div>
  {/if}
</div>

<style>
  .market-container {
    max-width: 100%;
  }

  .selection-grid {
    display: grid;
    grid-template-columns: repeat(2, 1fr);
    gap: 12px;
  }

  .chip-card {
    position: relative;
    cursor: pointer;
    background: white;
    border-radius: 16px;
    padding: 20px 10px;
    transition: all 0.2s ease;
    border: 2px solid transparent;
    box-shadow: 0 4px 12px rgba(0,0,0,0.05);
    text-align: center;
  }

  .chip-card input {
    position: absolute;
    opacity: 0;
    cursor: pointer;
  }

  .chip-content {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 8px;
  }

  .chip-icon {
    font-size: 1.5rem;
  }

  .chip-label {
    font-weight: 600;
    color: #495057;
    font-size: 0.9rem;
  }

  .chip-card.selected {
    border-color: #007bff;
    background: #f0f7ff;
    transform: translateY(-2px);
    box-shadow: 0 6px 15px rgba(0, 123, 255, 0.15);
  }

  .chip-card.selected .chip-label {
    color: #007bff;
  }

  @media (max-width: 400px) {
    .selection-grid {
      grid-template-columns: 1fr;
    }
    .chip-card {
        padding: 15px;
    }
    .chip-content {
        flex-direction: row;
        justify-content: center;
        gap: 15px;
    }
  }
</style>

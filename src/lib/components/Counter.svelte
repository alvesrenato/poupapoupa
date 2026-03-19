<script>
  import EnviarEmail from "./EnviarEmail.svelte";
  import EnviarWhatsApp from "./EnviarWhatsApp.svelte";

  // Svelte 5 Runes for better state management
  let saldo = $state("");
  let saldoIndefinido = $state(false);
  let caixas = $state([]);
  let ultimaCaixaInput = $state(null);

  // Derived values (computed)
  let saldoNumerico = $derived(parseFloat(saldo.replace(",", ".")) || 0);

  let valorGasto = $derived(
    caixas.reduce(
      (total, caixa) => total + (caixa.quantidade * (parseFloat(String(caixa.valorUnitario).replace(",", ".")) || 0)),
      0
    )
  );

  let restante = $derived(saldoNumerico - valorGasto);

  // Effect to focus the last input when a new item is added
  $effect(() => {
    if (caixas.length > 0 && ultimaCaixaInput) {
      ultimaCaixaInput.focus();
    }
  });

  function adicionarCaixa() {
    caixas.push({ quantidade: 1, valorUnitario: "" });
  }

  function removerCaixa(index) {
    caixas.splice(index, 1);
  }

  function handleKeyPress(e) {
    if (e.key === "Enter") {
      adicionarCaixa();
    }
  }
</script>

<div class="counter-container">
  <!-- Status Cards (Fixed/Sticky at the top) -->
  <div class="status-sticky-container">
    <div class="status-grid">
      {#if !saldoIndefinido}
        <div class="status-card balance">
          <span class="status-label">Saldo</span>
          <span class="status-value">€ {saldoNumerico.toFixed(2)}</span>
        </div>
      {/if}
      
      <div class="status-card spent">
        <span class="status-label">Gasto</span>
        <span class="status-value">€ {valorGasto.toFixed(2)}</span>
      </div>

      {#if !saldoIndefinido}
        <div class="status-card remaining" class:negative={restante < 0}>
          <span class="status-label">Restante</span>
          <span class="status-value">€ {restante.toFixed(2)}</span>
        </div>
      {/if}
    </div>
  </div>

  <div class="scroll-content">
    <!-- Saldo Input Card -->
  {#if !saldoIndefinido}
    <div class="card p-3 mb-4">
      <label for="saldoInput" class="form-label fw-bold">Quanto quer gastar hoje?</label>
      <div class="input-group">
        <span class="input-group-text bg-primary text-white border-0">€</span>
        <input
          id="saldoInput"
          type="text"
          inputmode="decimal"
          placeholder="0.00"
          bind:value={saldo}
          class="form-control"
        />
        <button class="btn btn-light border" onclick={() => (saldo = "")}>Zerar</button>
        <button class="btn btn-light border" onclick={() => (saldoIndefinido = true)}>Não sei</button>
      </div>
    </div>
  {:else}
    <div class="card p-3 mb-4 text-center">
      <button class="btn btn-link text-decoration-none" onclick={() => (saldoIndefinido = false)}>
        Definir um saldo limite?
      </button>
    </div>
  {/if}

  <!-- Items List -->
  <div class="items-section mb-4">
    <div class="d-flex justify-content-between align-items-center mb-3 px-1">
      <h3 class="h5 mb-0 fw-bold">Itens da Compra</h3>
      <span class="badge bg-secondary rounded-pill">{caixas.length} itens</span>
    </div>

    {#if caixas.length === 0}
      <div class="text-center py-5 bg-white rounded-4 border-dashed">
        <p class="text-muted mb-0">Nenhum item adicionado ainda.</p>
      </div>
    {:else}
      {#each caixas as caixa, index}
        <div class="item-card animate__animated animate__fadeInUp">
          <div class="row g-2 align-items-center">
            <div class="col-3">
              <select bind:value={caixa.quantidade} class="form-select border-0 bg-light">
                {#each Array(20) as _, i}
                  <option value={i + 1}>{i + 1}x</option>
                {/each}
              </select>
            </div>
            <div class="col-6">
              <div class="input-group">
                <span class="input-group-text bg-transparent border-0 pe-1">€</span>
                {#if index === caixas.length - 1}
                  <input
                    class="form-control border-0 bg-light"
                    type="text"
                    inputmode="decimal"
                    placeholder="Valor"
                    bind:this={ultimaCaixaInput}
                    bind:value={caixa.valorUnitario}
                    onkeypress={handleKeyPress}
                  />
                {:else}
                  <input
                    class="form-control border-0 bg-light"
                    type="text"
                    inputmode="decimal"
                    placeholder="Valor"
                    bind:value={caixa.valorUnitario}
                  />
                {/if}
              </div>
            </div>
            <div class="col-3 text-end">
              <button class="btn btn-link text-danger p-0" onclick={() => removerCaixa(index)}>
                ✕
              </button>
            </div>
          </div>
        </div>
      {/each}
    {/if}
  </div>

  <!-- Actions -->
  <div class="d-grid gap-2 mb-4">
    <button class="btn btn-primary btn-lg py-3 shadow-sm" onclick={adicionarCaixa}>
      <span class="me-2">➕</span> Adicionar Item
    </button>
    {#if caixas.length > 0}
      <button class="btn btn-light border-0 text-muted" onclick={() => confirm("Limpar tudo?") && (caixas = [])}>
        Limpar Lista
      </button>
    {/if}
  </div>

  <!-- Share -->
  <div class="card p-3 bg-light border-0">
    <p class="text-center text-muted small mb-2">Compartilhar resumo:</p>
    <div class="d-flex justify-content-center gap-3">
      <EnviarEmail valorGasto={valorGasto.toFixed(2)} />
      <EnviarWhatsApp valorGasto={valorGasto.toFixed(2)} />
    </div>
  </div>
</div> <!-- closes scroll-content -->
</div> <!-- closes counter-container -->

<style>
  .counter-container {
    max-width: 100%;
  }

  .status-sticky-container {
    position: sticky;
    top: 0; /* Sticks to the top of its parent */
    z-index: 100;
    background-color: #f4f6f9; /* Match body background */
    padding: 15px 0 15px 0;
    margin: 0;
  }

  .status-grid {
    display: grid;
    grid-template-columns: repeat(3, 1fr); /* Always 3 columns */
    gap: 8px;
  }

  .status-card {
    background: white;
    padding: 12px;
    border-radius: 16px;
    display: flex;
    flex-direction: column;
    align-items: center;
    box-shadow: 0 4px 12px rgba(0,0,0,0.05);
  }

  .status-label {
    font-size: 0.7rem;
    color: #6c757d;
    text-transform: uppercase;
    font-weight: 700;
    margin-bottom: 4px;
  }

  .status-value {
    font-size: 1.1rem;
    font-weight: 800;
    color: #2c3e50;
  }

  .remaining .status-value {
    color: #28a745;
  }

  .remaining.negative .status-value {
    color: #dc3545;
  }

  .item-card {
    background: white;
    padding: 12px;
    border-radius: 12px;
    margin-bottom: 10px;
    box-shadow: 0 2px 6px rgba(0,0,0,0.03);
  }

  .border-dashed {
    border: 2px dashed #e0e0e0;
  }
    input#saldoInput.form-control{
    border-radius: 0px 12px 12px 0px !important;
    
}
</style>

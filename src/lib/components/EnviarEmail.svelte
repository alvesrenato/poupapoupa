<script>
  import { nomeMercado } from '../stores/store.js';

  export let valorGasto;

  $: mercado = $nomeMercado;

  function compartilharPorEmail() {
    const datahoraAtual = new Date().toLocaleString('pt-PT', {
      year: 'numeric',
      month: '2-digit',
      day: '2-digit',
      hour: '2-digit',
      minute: '2-digit'
    });

    const assunto = encodeURIComponent(`Gasto no ${mercado || 'Estabelecimento'}`);
    const corpo = encodeURIComponent(
      `🎉 Resumo de Gastos 🎉\n\n` +
      `Data: ${datahoraAtual}\n` +
      `Local: ${mercado || 'Não especificado'}\n` +
      `Valor Gasto: € ${valorGasto}\n\n` +
      `Gerado por PoupaPoupa. 💸`
    );

    window.open(`mailto:?subject=${assunto}&body=${corpo}`, '_blank');
  }
</script>

<button
  class="btn btn-email"
  on:click={compartilharPorEmail}
>
  📧 Email
</button>

<style>
  .btn-email {
    background-color: #f8f9fa;
    border: 1px solid #e0e0e0;
    color: #495057;
    font-weight: 600;
    padding: 8px 20px;
    border-radius: 100px;
    transition: all 0.2s;
  }

  .btn-email:hover {
    background-color: #e9ecef;
    transform: translateY(-1px);
  }
</style>

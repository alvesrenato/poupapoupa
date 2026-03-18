<script>
  import { nomeMercado } from '../stores/store.js';

  export let valorGasto;

  $: mercado = $nomeMercado;

  function compartilharViaWhatsApp() {
    const datahoraAtual = new Date().toLocaleString('pt-PT', {
      year: 'numeric',
      month: '2-digit',
      day: '2-digit',
      hour: '2-digit',
      minute: '2-digit'
    });

    const texto = encodeURIComponent(
      `🎉 Detalhes do Gasto 🎉\n\n` +
      `Data: ${datahoraAtual}\n` +
      `Valor Gasto: € ${valorGasto}\n` +
      `Local: ${mercado || 'Não especificado'}\n\n` +
      `PoupaPoupa 💸`
    );

    window.open(`https://wa.me/?text=${texto}`, '_blank');
  }
</script>

<button
  class="btn btn-whatsapp"
  on:click={compartilharViaWhatsApp}
>
  🟢 WhatsApp
</button>

<style>
  .btn-whatsapp {
    background-color: #25d366;
    color: white;
    font-weight: 600;
    padding: 8px 20px;
    border-radius: 100px;
    border: none;
    transition: all 0.2s;
  }

  .btn-whatsapp:hover {
    background-color: #1da851;
    transform: translateY(-1px);
    box-shadow: 0 4px 10px rgba(37, 211, 102, 0.3);
  }
</style>

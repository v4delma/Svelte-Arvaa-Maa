<script>
  import 'papercss/dist/paper.min.css';

  let aloitaPeli = false;
  let oikeatVastaukset = 0;
  let vaaratVastaukset = 0;
  let nimi = '';
  $: nimiKelpaa = nimi.trim().length > 0;
  let arvattavaMaa = '';
  let arvattavanMaanLippu = '';

  const maat = [
    'Russia',
    'Germany',
    'United Kingdom',
    'France',
    'Italy',
    'Spain',
    'Ukraine',
    'Poland',
    'Romania',
    'Netherlands',
    'Belgium',
    'Czechia',
    'Greece',
    'Portugal',
    'Sweden',
    'Hungary',
    'Belarus',
    'Austria',
    'Serbia',
    'Switzerland',
    'Bulgaria',
    'Denmark',
    'Finland',
    'Slovakia',
    'Norway',
    'Ireland',
    'Croatia',
    'Moldova',
    'Bosnia and Herzegovina',
    'Albania',
  ];

  function arvoMaa() {
    arvattavaMaa = maat[Math.floor(Math.random() * maat.length)];
  }

  async function haeLippu() {
    const response = await fetch(
      'https://restcountries.com/v3.1/name/' + arvattavaMaa
    );
    const json = await response.json();
    arvattavanMaanLippu = json[0].flags.png;
  }

  let vaihtoehto1 = '';
  let vaihtoehto2 = '';
  let vaihtoehto3 = '';

  function arvoVaihtoehdot() {
    while (
      vaihtoehto1 == vaihtoehto2 ||
      vaihtoehto2 == vaihtoehto3 ||
      vaihtoehto1 == vaihtoehto3 ||
      vaihtoehto1 == arvattavaMaa ||
      vaihtoehto2 == arvattavaMaa ||
      vaihtoehto3 == arvattavaMaa
    ) {
      vaihtoehto1 = maat[Math.floor(Math.random() * maat.length)];
      vaihtoehto2 = maat[Math.floor(Math.random() * maat.length)];
      vaihtoehto3 = maat[Math.floor(Math.random() * maat.length)];
    }
  }

  let vaihtoehdot = [];
  function sekoitaVaihtoehdot() {
    vaihtoehdot = [vaihtoehto1, vaihtoehto2, vaihtoehto3, arvattavaMaa];
    vaihtoehdot.sort(() => Math.random() - 0.5);
  }

  function arvaus(event) {
    if (event.target.textContent == arvattavaMaa) {
      alert('Oikein!');
      oikeatVastaukset++;
      seuraava();
    } else {
      alert('Väärin!');
      vaaratVastaukset++;
    }
  }

  function aloita() {
    arvoMaa();
    haeLippu();
    arvoVaihtoehdot();
    sekoitaVaihtoehdot();
    oikeatVastaukset = 0;
    vaaratVastaukset = 0;
    aloitaPeli = true;
  }

  function seuraava() {
    arvoMaa();
    haeLippu();
    arvoVaihtoehdot();
    sekoitaVaihtoehdot();
  }
</script>

<main>
  <div>
    <h1>Arvaa maa</h1>
    {#if !aloitaPeli}
      <div>
        <label for="nimi">Pelaajan nimi:</label>
        <br />
        <input type="text" bind:value={nimi} id="nimi" />
        <br />
        <button on:click={aloita} disabled={!nimiKelpaa}>Aloita peli</button>
      </div>
    {:else}
      <h5>Pelaaja: {nimi}</h5>
      <h6>Oikeat vastaukset: {oikeatVastaukset}</h6>
      <h6>Väärät vastaukset: {vaaratVastaukset}</h6>
      <img src={arvattavanMaanLippu} alt="lippu" />
      <div class="napit">
        {#each vaihtoehdot as vaihtoehto}
          <button on:click={arvaus}>{vaihtoehto}</button>
        {/each}
      </div>
    {/if}
  </div>
</main>

<style>
  main {
    display: flex;
    justify-content: center;
  }

  .napit {
    margin-top: 10px;
  }

  button {
    margin: 5px;
    padding: 10px;
  }
</style>

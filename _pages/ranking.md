---
title: "Ranking"
permalink: /ranking/
author_profile: true
---

<div style="display:flex; justify-content:space-between; align-items:center; margin-bottom:20px;">
  <h2>Ranking Filippi Slam</h2>

  <p>
Il <strong>Ranking Filippi Slam</strong> rappresenta la classifica ufficiale dei partecipanti e premia sia la costanza nel prendere parte alle edizioni del torneo sia i risultati ottenuti sul campo. Il punteggio complessivo di ogni giocatore è dato dalla somma dei punti conquistati nelle diverse edizioni comprese nell'intervallo di tempo selezionato.
</p>

<p>
Per ogni torneo vengono assegnati:
</p>

<ul>
  <li><strong>20 punti</strong> per la partecipazione;</li>
  <li><strong>5 punti</strong> per ogni vittoria ottenuta nella fase a gironi;</li>
  <li><strong>15 punti</strong> per la qualificazione alle semifinali;</li>
  <li><strong>15 punti</strong> per il raggiungimento della finale;</li>
  <li><strong>20 punti</strong> per la vittoria del torneo.</li>
</ul>

<p>
Attraverso il menu a tendina in alto a destra è possibile visualizzare la classifica relativa agli ultimi <strong>1 mese</strong>, <strong>6 mesi</strong>, <strong>1 anno</strong>, <strong>3 anni</strong> oppure la <strong>classifica assoluta</strong>, che comprende tutte le edizioni disputate.
</p>

  <select id="ranking-filter" style="padding:6px 10px; border-radius:6px;">
    <option value="all" selected>Sempre</option>
    <option value="1">1 mese</option>
    <option value="6">6 mesi</option>
    <option value="12">1 anno</option>
    <option value="36">3 anni</option>
  </select>
</div>

<table id="ranking-table">
  <thead>
    <tr>
      <th>Pos.</th>
      <th>Giocatore</th>
      <th>Punti</th>
    </tr>
  </thead>
  <tbody></tbody>
</table>

<script>
const players = {{ site.data.players | jsonify }};
const editions = {{ site.data.editions | jsonify }};

function parseItalianDate(dateString) {
  const mesi = {
    "gennaio": 0,
    "febbraio": 1,
    "marzo": 2,
    "aprile": 3,
    "maggio": 4,
    "giugno": 5,
    "luglio": 6,
    "agosto": 7,
    "settembre": 8,
    "ottobre": 9,
    "novembre": 10,
    "dicembre": 11
  };

  const parti = dateString.toLowerCase().split(" ");
  const giorno = parseInt(parti[0]);
  const mese = mesi[parti[1]];
  const anno = parseInt(parti[2]);

  return new Date(anno, mese, giorno);
}

function getPlayerName(slug) {
  const player = players.find(p => p.slug === slug);
  return player ? player.nome : slug;
}

function calculateRanking(months) {
  const today = new Date();
  let minDate = null;

  if (months !== "all") {
    minDate = new Date(today);
    minDate.setMonth(minDate.getMonth() - parseInt(months));
  }

  let ranking = {};

  editions.forEach(edizione => {
    if (!edizione.punti) return;

    const editionDate = parseItalianDate(edizione.data);

    if (minDate && editionDate < minDate) return;

    Object.entries(edizione.punti).forEach(([slug, punti]) => {
      if (!ranking[slug]) {
        ranking[slug] = {
          slug: slug,
          nome: getPlayerName(slug),
          punti: 0
        };
      }

      ranking[slug].punti += punti;
    });
  });

  return Object.values(ranking).sort((a, b) => b.punti - a.punti);
}

function renderRanking() {
  const filter = document.getElementById("ranking-filter").value;
  const ranking = calculateRanking(filter);
  const tbody = document.querySelector("#ranking-table tbody");

  tbody.innerHTML = "";

  ranking.forEach((giocatore, index) => {
    tbody.innerHTML += `
      <tr>
        <td>${index + 1}</td>
        <td>
          <a href="/giocatore/?id=${giocatore.slug}">
            ${giocatore.nome}
          </a>
        </td>
        <td><strong>${giocatore.punti}</strong></td>
      </tr>
    `;
  });

  if (ranking.length === 0) {
    tbody.innerHTML = `
      <tr>
        <td colspan="3">Nessun risultato disponibile per il periodo selezionato.</td>
      </tr>
    `;
  }
}

document.getElementById("ranking-filter").addEventListener("change", renderRanking);

renderRanking();
</script>

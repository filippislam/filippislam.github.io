---
title: "Edizioni"
permalink: /edizioni/
author_profile: true
---

<h2>Edizioni del Filippi Slam</h2>

<table>
  <thead>
    <tr>
      <th>Edizione</th>
      <th>Data</th>
      <th>Partecipanti</th>
    </tr>
  </thead>
  <tbody id="edizioni-table"></tbody>
</table>

<script>
  const editions = {{ site.data.editions | jsonify }};

  let html = "";

  editions.forEach(edition => {
    html += `
      <tr>
        <td>
          <a href="/edizione/?id=${edition.slug}">
            ${edition.nome}
          </a>
        </td>
        <td>${edition.data}</td>
        <td>${edition.partecipanti ? edition.partecipanti.length : 0}</td>
      </tr>
    `;
  });

  document.getElementById("edizioni-table").innerHTML = html;
</script>

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
  <tbody>

    {% for edition in site.data.editions %}
    <tr>
      <td>
        <a href="/edizioni/{{ edition.slug }}/">
          {{ edition.nome }}
        </a>
      </td>
      <td>{{ edition.data }}</td>
      <td>{{ edition.partecipanti.size }}</td>
    </tr>
    {% endfor %}

  </tbody>
</table>

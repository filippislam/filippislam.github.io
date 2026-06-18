---
title: "Partecipanti"
permalink: /partecipanti/
author_profile: true
---

In questa sezione sono riportati tutti i partecipanti che hanno preso parte ad almeno un'edizione del Filippi Slam nel corso degli anni.<br>

<table>
  <thead>
    <tr>
      <th>Nome</th>
      <th>Braccio</th>
      <th>Classe</th>
      <th>Genere</th>
    </tr>
  </thead>
  <tbody>
    {% for player in site.data.players %}
    <tr>
      <td>{{ player.nome }}</td>
      <td>{{ player.braccio }}</td>
      <td>{{ player.anno }}</td>
      <td>{{ player.genere }}</td>
    </tr>
    {% endfor %}
  </tbody>
</table>

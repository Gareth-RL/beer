---
layout: paginated_index
title: Beer Ratings Directory
---

# Beer Ratings Directory

Here are the beers I reviewed using ratebeer

---

## Search
<input type="text" id="searchBox" onkeyup="filterBeers()" placeholder="Search for beers..." style="width: 100%; padding: 10px; font-size: 16px;">

<table id="resultsTable" style="width: 100%; border-collapse: collapse; margin-top: 20px;">
  <thead>
    <tr style="background-color: #f4f4f4;">
      <th style="border: 1px solid #ddd; padding: 8px; text-align: left;">Beer Name</th>
      <th style="border: 1px solid #ddd; padding: 8px; text-align: left;">Brewer</th>
      <th style="border: 1px solid #ddd; padding: 8px; text-align: left;">Style</th>
      <th style="border: 1px solid #ddd; padding: 8px; text-align: left;">Country</th>
      <th style="border: 1px solid #ddd; padding: 8px; text-align: left;">Rating</th>
    </tr>
  </thead>
  <tbody>
    <!-- Results will be dynamically inserted here -->
  </tbody>
</table>

<script>
  // Fetch beers data and populate the table
  fetch('./beers.json')
    .then(response => response.json())
    .then(beers => {
      const tableBody = document.querySelector('#resultsTable tbody');

      beers.forEach(beer => {
        const row = document.createElement('tr');
        row.innerHTML = `
          <td style="border: 1px solid #ddd; padding: 8px;">
            <a href="${beer.link}">${beer.beer}</a>
          </td>
          <td style="border: 1px solid #ddd; padding: 8px;">${beer.brewer}</td>
          <td style="border: 1px solid #ddd; padding: 8px;">${beer.style}</td>
          <td style="border: 1px solid #ddd; padding: 8px;">${beer.country}</td>
          <td style="border: 1px solid #ddd; padding: 8px;">${beer.rating}</td>
        `;
        tableBody.appendChild(row);
      });
    });

  // Filter results based on user input
  function filterBeers() {
    const searchInput = document.getElementById('searchBox').value.toLowerCase();
    const tableRows = document.querySelectorAll('#resultsTable tbody tr');

    tableRows.forEach(row => {
      const rowText = row.innerText.toLowerCase();
      row.style.display = rowText.includes(searchInput) ? '' : 'none';
    });
  }
</script>

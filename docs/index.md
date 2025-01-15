---
layout: default
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
  let beersData = []; // Store the beers data globally

  // Fetch beers data and store it for filtering
  fetch('./beers.json')
    .then(response => response.json())
    .then(beers => {
      beersData = beers; // Store the data globally
    });

  // Filter results based on user input
  function filterBeers() {
    const searchInput = document.getElementById('searchBox').value.toLowerCase();
    const tableBody = document.querySelector('#resultsTable tbody');
    tableBody.innerHTML = ''; // Clear previous results

    if (searchInput.length < 2) {
      // Show no results if the search input is less than 2 characters
      tableBody.innerHTML = '<tr><td colspan="5" style="text-align: center;">Please enter at least 2 characters to search</td></tr>';
      return;
    }

    // Filter beers based on the search input
    const filteredBeers = beersData.filter(beer => {
      const rowText = `${beer.beer} ${beer.brewer} ${beer.style} ${beer.country} ${beer.rating}`.toLowerCase();
      return rowText.includes(searchInput);
    });

    // Show up to 50 results
    const limitedBeers = filteredBeers.slice(0, 50);

    if (limitedBeers.length === 0) {
      tableBody.innerHTML = '<tr><td colspan="5" style="text-align: center;">No results found</td></tr>';
      return;
    }

    // Populate the table with filtered results
    limitedBeers.forEach(beer => {
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
  }
</script>

<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1" />
<title>Search & Highlight</title>
<style>
  body {
    font-family: Arial, sans-serif;
    max-width: 700px;
    margin: 40px auto;
    padding: 0 10px;
  }
  h2 {
    font-weight: bold;
  }
  input[type="search"] {
    width: 100%;
    padding: 8px 12px;
    font-size: 16px;
    border-radius: 4px;
    border: 1px solid #ccc;
    margin-bottom: 20px;
  }
  mark {
    background-color: #ffeb3b;
    padding: 0;
  }
  article {
    margin-bottom: 40px;
  }
  h3 {
    font-weight: bold;
    font-size: 20px;
    line-height: 1.2;
  }
  p.date {
    font-style: italic;
    color: #555;
    margin-top: 4px;
    margin-bottom: 8px;
  }
  p.excerpt {
    line-height: 1.5;
    font-size: 15px;
    color: #333;
  }
  hr {
    margin-top: 24px;
    border-color: #eee;
  }
</style>
</head>
<body>

<h2>Search</h2>
<input type="search" id="searchInput" placeholder="Search articles..." aria-label="Search articles" />

<p><strong id="resultCount">0</strong> posts were found.</p>

<div id="results"></div>

<script>
  // Articles array
  const articles = [
    {
      title: "Understanding the difference between grid-template and grid-auto",
      date: "Oct 09, 2018",
      excerpt: "With all the new properties related to CSS Grid Layout, one of the distinctions that always confused me was the difference between the grid-template-* and grid-auto-* properties. Specifically the difference between grid-template-rows/columns and grid-auto-rows/columns. Although I knew they were to d..."
    },
    {
      title: "Recreating the GitHub Contribution Graph with CSS Grid Layout",
      date: "Jan 15, 2019",
      excerpt: "Learn how to recreate the GitHub contribution graph using modern CSS Grid Layout, making it flexible and responsive for all screen sizes."
    },
    {
      title: "CSS Grid: A Comprehensive Guide",
      date: "Dec 01, 2017",
      excerpt: "This article provides a comprehensive guide on CSS Grid, including tutorials, examples, and best practices to master grid layout techniques."
    },
  ];

  const searchInput = document.getElementById("searchInput");
  const resultsDiv = document.getElementById("results");
  const resultCount = document.getElementById("resultCount");

  // Escape special regex characters in search query
  function escapeRegExp(string) {
    return string.replace(/[.*+?^${}()|[\]\\]/g, '\\$&');
  }

  // Highlight query matches in text
  function highlightText(text, query) {
    if (!query.trim()) return text;
    const escapedQuery = escapeRegExp(query);
    const regex = new RegExp(`(${escapedQuery})`, "gi");
    return text.replace(regex, '<mark>$1</mark>');
  }

  // Render filtered articles with highlighted matches
  function renderResults(query) {
    const filtered = articles.filter(({title, excerpt}) =>
      title.toLowerCase().includes(query.toLowerCase()) || excerpt.toLowerCase().includes(query.toLowerCase())
    );

    resultCount.textContent = filtered.length;

    if (filtered.length === 0) {
      resultsDiv.innerHTML = '<p>No posts found.</p>';
      return;
    }

    resultsDiv.innerHTML = filtered.map(({title, date, excerpt}) => `
      <article>
        <h3>${highlightText(title, query)}</h3>
        <p class="date">${date}</p>
        <p class="excerpt">${highlightText(excerpt, query)}</p>
        <hr/>
      </article>
    `).join("");
  }

  // Listen for input event to update results dynamically
  searchInput.addEventListener("input", e => renderResults(e.target.value));

  // Initial render to show all articles
  renderResults("");
</script>

</body>
</html>

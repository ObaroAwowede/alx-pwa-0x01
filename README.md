## API Overview
  <p>
    The MoviesDatabase API is a RESTful service hosted on RapidAPI that provides comprehensive, up‑to‑date metadata on over 9 million titles (movies, series, and episodes) and 11 million people (actors, directors, and crew). Key features include keyword‑based searches, full title lookups (with trailers, ratings, awards, and more), detailed person profiles, navigation through series → seasons → episodes, and utility endpoints for genres, title types, and curated “top” lists.
  </p>

## Version
  <p><strong>v1</strong></p>

## Available Endpoints

  <h3>Titles</h3>
  <ul>
    <li><code>GET /titles</code><br/>Returns multiple titles based on filters/sorting (e.g., genre, year, rating).</li>
    <li><code>POST /x/titles-by-ids</code><br/>Fetches multiple titles by an array of IMDb IDs.</li>
    <li><code>GET /titles/{id}</code><br/>Retrieves detailed information for a specific title.</li>
    <li><code>GET /titles/{id}/ratings</code><br/>Retrieves the rating and vote count for a title.</li>
    <li><code>GET /titles/series/{id}</code><br/>Lists all episodes for a TV series.</li>
    <li><code>GET /titles/seasons/{id}</code><br/>Returns the number of seasons for a series.</li>
    <li><code>GET /titles/series/{id}/{season}</code><br/>Lists the episodes in a specific season.</li>
    <li><code>GET /titles/episode/{id}</code><br/>Retrieves details of a particular episode.</li>
    <li><code>GET /titles/x/upcoming</code><br/>Lists upcoming titles (movies/episodes).</li>
  </ul>

  <h3>Search</h3>
  <ul>
    <li><code>GET /titles/search/keyword/{keyword}</code><br/>Searches titles by a keyword.</li>
    <li><code>GET /titles/search/title/{title}</code><br/>Searches titles by full or partial title text (supports <code>exact</code> query param).</li>
    <li><code>GET /titles/search/akas/{aka}</code><br/>Searches titles by alternate title (exact, case‑sensitive).</li>
  </ul>

  <h3>Actors</h3>
  <ul>
    <li><code>GET /actors</code><br/>Returns a list of actors with optional <code>limit</code> and <code>page</code> filters.</li>
    <li><code>GET /actors/{id}</code><br/>Retrieves detailed information about a specific actor.</li>
  </ul>

  <h3>Utils</h3>
  <ul>
    <li><code>GET /title/utils/titleType</code><br/>Returns the valid title types (e.g., “movie”, “tvSeries”, etc.).</li>
    <li><code>GET /title/utils/genres</code><br/>Retrieves the list of supported genres.</li>
    <li><code>GET /title/utils/lists</code><br/>Returns predefined list names for the <code>list</code> query parameter (e.g., “top_rated”, “most_popular”).</li>
  </ul>

## Request and Response Format
  <p>All requests and responses use JSON. Below is an example of searching for titles by keyword:</p>

  <h3>Request</h3>
  <pre><code>GET /titles/search/keyword/inception?limit=5&amp;page=1 HTTP/1.1
    Host: moviesdatabase.p.rapidapi.com
    X-RapidAPI-Key: YOUR_API_KEY
    X-RapidAPI-Host: moviesdatabase.p.rapidapi.com
  </code></pre>

  <h3>Sample Response</h3>
  <pre><code>{
  "results": [
    {
      "id": "tt1375666",
      "titleText": { "text": "Inception" },
      "releaseYear": { "year": 2010 },
      "ratingsSummary": {
        "aggregateRating": 8.8,
        "voteCount": 2100000
      }
    }
    // ...additional results...
  ],
  "page": 1,
  "total_pages": 10,
  "total_results": 50
}
  </code></pre>
  <p>Optional query parameters include <code>limit</code>, <code>page</code>, <code>info</code>, <code>sort</code>, <code>genre</code>, <code>startYear</code>, etc.</p>

## Authentication
  <p>Requests must include these headers:</p>
  <pre><code>X-RapidAPI-Key: &lt;YOUR_RAPIDAPI_KEY&gt;
    X-RapidAPI-Host: moviesdatabase.p.rapidapi.com
  </code></pre>
  <p>Obtain your key by subscribing on the RapidAPI dashboard.</p>

## Error Handling
  <ul>
    <li><strong>400 Bad Request</strong> – Invalid or missing parameters.</li>
    <li><strong>401 Unauthorized</strong> – Missing or invalid API key.</li>
    <li><strong>403 Forbidden</strong> – Subscription doesn’t include this endpoint.</li>
    <li><strong>404 Not Found</strong> – Resource does not exist.</li>
    <li><strong>429 Too Many Requests</strong> – Rate limit exceeded.</li>
    <li><strong>500 Internal Server Error</strong> – Retry after a short delay.</li>
  </ul>

## Usage Limits and Best Practices
  <ul>
    <li><strong>Rate Limits:</strong> Set by your RapidAPI plan (e.g., 50–500 calls/minute).</li>
    <li><strong>Pagination:</strong> Use <code>limit</code> and <code>page</code> to navigate large datasets.</li>
    <li><strong>Caching:</strong> Cache static data (genres, lists).</li>
    <li><strong>Error Backoff:</strong> Implement exponential backoff on <code>429</code> or <code>5xx</code> errors.</li>
    <li><strong>Efficient <code>info</code> Usage:</strong> Specify only the detail level you need to reduce payload size.</li>
  </ul>

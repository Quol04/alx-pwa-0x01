# alx-project-0x14

CineSeek is a modern movie discovery application built with Next.js, TypeScript, and Tailwind CSS. The application allows users to browse movies from the MoviesDatabase API, view movie details, and search for films by year or genre.

---

# ALX Project 0x14 — Using the MoviesDatabase API

## API Overview
The **MoviesDatabase API** provides a vast collection of information about **movies, TV shows, and actors**, including YouTube trailer URLs, awards, full biographies, and many other useful details.  
It offers **complete and updated data** for over **9 million titles** (movies, series, and episodes) and **11 million actors, crew, and cast members**.

**Key Highlights:**
- Over 9 million movie, series, and episode entries  
- Over 11 million actor and crew records  
- Weekly updates for new titles and daily updates for ratings and episodes  
- Includes trailers, ratings, awards, and biography data  
- Developer support: [Buy Me a Coffee](https://www.buymeacoffee.com/SAdrian13)

---

## API Version
**Version:** v1  
**Updates:**  
- Titles updated weekly  
- Ratings and episodes updated daily  

---

## Available Endpoints

| Category | Endpoint | Description |
|-----------|-----------|-------------|
| **Titles** | `/titles` | Returns an array of titles according to filters and sorting parameters. |
|  | `/x/titles-by-ids` | Returns titles based on an array of IMDb IDs. |
|  | `/titles/{id}` | Fetches detailed info about a specific title using IMDb ID. |
|  | `/titles/{id}/ratings` | Retrieves rating and votes count for a given title. |
|  | `/titles/series/{id}` | Returns all episodes of a series (IDs, episode, and season numbers). |
|  | `/titles/seasons/{id}` | Returns the number of seasons for a given series. |
|  | `/titles/series/{id}/{season}` | Returns all episode IDs for a specific season. |
|  | `/titles/episode/{id}` | Retrieves episode details by IMDb ID. |
|  | `/titles/x/upcoming` | Returns a list of upcoming titles. |
| **Search** | `/titles/search/keyword/{keyword}` | Search titles using keywords. |
|  | `/titles/search/title/{title}` | Search titles by name (exact or partial match). |
|  | `/titles/search/akas/{aka}` | Search titles by alternative names (exact match only). |
| **Actors** | `/actors` | Returns an array of actors according to filters. |
|  | `/actors/{id}` | Returns detailed information about a specific actor. |
| **Utils** | `/title/utils/titleType` | Returns available title types. |
|  | `/title/utils/genres` | Returns all available genres. |
|  | `/title/utils/lists` | Returns predefined lists (e.g., top-rated, most popular). |

---

## Request and Response Format

### Request Example
```typescript
fetch("https://moviesdatabase.p.rapidapi.com/titles/search/title/Inception", {
  method: "GET",
  headers: {
    "x-rapidapi-key": "YOUR_API_KEY",
    "x-rapidapi-host": "moviesdatabase.p.rapidapi.com"
  }
});
```

### Response Example
```json
{
  "results": [
    {
      "id": "tt1375666",
      "titleText": { "text": "Inception" },
      "releaseYear": { "year": 2010 },
      "ratingsSummary": { "aggregateRating": 8.8 },
      "primaryImage": {
        "url": "https://image.url/inception.jpg"
      }
    }
  ],
  "page": 1,
  "next": "/titles?page=2",
  "entries": 50
}
```

### TypeScript Interface Example
```typescript
export interface Title {
  id: string;
  titleText: { text: string };
  releaseYear: { year: number };
  ratingsSummary: { aggregateRating: number };
  primaryImage?: { url: string };
}
```

---

## Authentication
The MoviesDatabase API requires an **API Key** for all requests.

### Authentication Headers:
```json
{
  "x-rapidapi-key": "YOUR_API_KEY",
  "x-rapidapi-host": "moviesdatabase.p.rapidapi.com"
}
```

You can get your key by signing up on [RapidAPI](https://rapidapi.com/).

---

## Error Handling

| Status Code | Meaning | Description |
|--------------|----------|-------------|
| **400** | Bad Request | Invalid or missing parameters. |
| **401** | Unauthorized | Invalid or missing API key. |
| **403** | Forbidden | Access to the resource is denied. |
| **404** | Not Found | The requested resource could not be found. |
| **429** | Too Many Requests | Exceeded the API rate limit. |
| **500** | Server Error | Internal server error occurred. |

### Example Error Handling in TypeScript
```typescript
try {
  const res = await fetch(url, options);
  if (!res.ok) {
    throw new Error(`Error ${res.status}: ${res.statusText}`);
  }
  const data = await res.json();
} catch (err) {
  console.error("API Error:", err.message);
}
```

---

## Usage Limits and Best Practices

**Usage Limits:**
- Free tier: ~500 requests/month  
- Higher tiers available via RapidAPI  
- Exceeding the limit results in `429 Too Many Requests`

**Best Practices:**
- Cache results locally to minimize repeated requests.
- Use pagination (`page` parameter) to manage large data sets.
- Implement exponential backoff when retrying failed requests.
- Validate API data before displaying.
- Keep your API key private and never expose it in client-side code.

---

## Example Optional Query Parameters

| Parameter | Type | Description |
|------------|------|-------------|
| `info` | string | Defines which information set to return (e.g., `base_info`, `mini_info`, `rating`, `awards`). |
| `limit` | number | Number of objects per page (max 50). |
| `page` | number | Page number for paginated results. |
| `genre` | string | Filters titles by genre (case-sensitive). |
| `startYear` | number | Filters results from a starting year. |
| `endYear` | number | Filters results to an ending year. |
| `titleType` | string | Type of title (from `/title/utils/titleType`). |
| `sort` | string | Sorting options such as `year.incr` or `year.decr`. |
| `list` | string | Select predefined lists like `most_pop_movies`, `top_rated_250`. |

---

## Data Models

### Rating Model
```json
{
  "tconst": "tt0000003",
  "averageRating": 6.5,
  "numVotes": 1631
}
```

### Light Episode Model
```json
{
  "tconst": "tt0020666",
  "parentTconst": "tt15180956",
  "seasonNumber": 1,
  "episodeNumber": 2
}
```

### Actor Model
```json
{
  "nconst": "nm0000001",
  "primaryName": "Fred Astaire",
  "birthYear": 1899,
  "deathYear": 1987,
  "primaryProfession": "soundtrack,actor,miscellaneous",
  "knownForTitles": "tt0050419,tt0053137,tt0072308,tt0031983"
}
```

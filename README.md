# Book Data API

> 💬 **Support, custom features or higher limits? Telegram: [@pintoflow](https://t.me/pintoflow)**

Search millions of books by **title, author or ISBN** and get clean, ready-to-use JSON: authors, description, publish date, page count, publisher, subjects, ISBN-10/13 and cover images in three sizes, plus author profiles with biography, photo and list of works.

**Get it here:**

- [Book Data API on RapidAPI](https://rapidapi.com/belchiorarkad-FqvHs2EDOtP/api/book-data-api)
- [Book Data API on Zyla API Hub](https://zylalabs.com/api-marketplace/other/book+data+api/14977)

## Features

- **Search** by title, author or ISBN (ISBN-10 and ISBN-13 detected automatically, hyphens allowed)
- **Book detail** by work id or straight from an ISBN
- **Author detail** with bio, birth and death dates, photo and paginated works
- **Cover images** in small, medium and large
- One consistent JSON envelope for every response, and fields are never omitted (null or an empty list when unknown)
- Books in many languages, with a three-letter language code

## Endpoints

| Method | Path | What it does |
| ------ | ---- | ------------ |
| GET | `/books/search?q=&limit=&offset=` | Search by title, author or ISBN |
| GET | `/books/{workId}` | Full detail for a book (e.g. `OL66554W`) |
| GET | `/isbn/{isbn}` | Full detail for a book by ISBN-10 or ISBN-13 |
| GET | `/authors/{authorId}?limit=&offset=` | Author profile and their works (e.g. `OL21594A`) |
| GET | `/health` | Service status |

## Quick start

Subscribe on RapidAPI, copy your key and send it with every request:

```bash
curl -G "https://book-data-api.p.rapidapi.com/books/search" \
  --data-urlencode "q=pride and prejudice" -d limit=2 \
  -H "X-RapidAPI-Key: YOUR_RAPIDAPI_KEY" \
  -H "X-RapidAPI-Host: book-data-api.p.rapidapi.com"
```

Use the exact host shown in your RapidAPI playground if it differs.

## Example: look up a book by ISBN

```python
import requests

r = requests.get(
    "https://book-data-api.p.rapidapi.com/isbn/9780141439518",
    headers={
        "X-RapidAPI-Key": "YOUR_RAPIDAPI_KEY",
        "X-RapidAPI-Host": "book-data-api.p.rapidapi.com",
    },
    timeout=30,
)
book = r.json()["data"]
print(book["title"], book["publisher"], book["pageCount"])
```

Real response (shortened):

```json
{
"error": null,
"data": {
"id": "OL66554W",
"title": "Pride and Prejudice",
"authors": [{ "id": "OL21594A", "name": "Jane Austen" }],
"isbn10": "0141439513",
"isbn13": "9780141439518",
"publisher": "Penguin Books",
"publishDate": "2003",
"pageCount": 435,
"physicalFormat": "Paperback",
"language": "eng",
"coverUrl": {
"small": "https://covers.openlibrary.org/b/id/14348537-S.jpg",
"medium": "https://covers.openlibrary.org/b/id/14348537-M.jpg",
"large": "https://covers.openlibrary.org/b/id/14348537-L.jpg"
}
},
"message": "Fetched book metadata."
}
```

## Example: search results

```json
{
"error": null,
"data": {
"query": "pride and prejudice",
"count": 2,
"results": [{
"workId": "OL66554W",
"title": "Pride and Prejudice",
"authors": ["Jane Austen"],
"authorIds": ["OL21594A"],
"firstPublishYear": 1813,
"editionCount": 4042,
"coverId": 14348537
}]
},
"message": "Found 2 book(s) for \"pride and prejudice\". Call GET /books/:id (or /isbn/:isbn) for full metadata."
}
```

## Use cases

- **Barcode scanner / ISBN lookup:** scan the back of a book (EAN-13 is the ISBN-13) and call `/isbn/{code}` for title, author, cover and page count in one request
- **Search-as-you-type:** call `/books/search` with a small `limit` after the user pauses typing, then load the detail when they pick a result
- **Author pages:** take the author id from any book and call `/authors/{id}` for bio, photo and works
- **Library, bookshop and reading-tracker apps:** enrich a list of ISBNs with clean metadata and covers
- **Bulk enrichment:** loop over your ISBNs, retry on 502, treat 404 as not in the catalogue

## Errors

| Status | Meaning |
| ------ | ------- |
| 200 | Success |
| 400 | Bad input (missing q, invalid ISBN, malformed id) |
| 404 | No book, ISBN or author found |
| 502 | Catalogues could not be reached right now, retry after a few seconds |

## Support

Questions, bugs, feature requests or custom plans? Message me on Telegram: **[@pintoflow](https://t.me/pintoflow)**

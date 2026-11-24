# Finding Hidden URLs and Archived Content Using the Wayback Machine CDX API

The Internet Archive's Wayback Machine stores historical snapshots of websites. However, some archived URLs and historical resources may not be easily discoverable through the standard Wayback Machine interface.

You can use the **Wayback Machine CDX API** to enumerate archived URLs, timestamps, and HTTP status codes for a target domain.

## 1. Retrieve Archived URLs Using CDX API

Use the following URL:

```text
https://web.archive.org/cdx/search/cdx?url=xyz.com&output=json&from=2024&to=2025&fl=timestamp,original,statuscode
```

### Parameters Explained

| Parameter | Description |
|---|---|
| `url` | Target domain |
| `output=json` | Returns results in JSON format |
| `from` | Starting year |
| `to` | Ending year |
| `fl` | Fields to retrieve |
| `timestamp` | Date and time of the archived snapshot |
| `original` | Original URL captured by the archive |
| `statuscode` | HTTP response status code |

### Example Output

```json
[
  ["timestamp", "original", "statuscode"],
  ["20240115294742", "https://xyz.com/admin", "200"],
  ["20240218294742", "https://xyz.com/api", "200"],
  ["20240510294742", "https://xyz.com/backup", "403"],
  ["20240920294742", "https://xyz.com/config", "200"]
]
```

The response provides archived URLs along with their corresponding timestamps and HTTP status codes.

## 2. Retrieve Archived Website Content

Once you have identified an interesting archived URL, copy its timestamp and use the following URL structure:

```text
https://web.archive.org/web/TIMESTAMPid_/ORIGINAL_URL
```

### Example

Suppose the CDX API returns:

```text
Timestamp: 20240115294742
Original: https://xyz.com/admin
Status Code: 200
```

Construct the following URL:

```text
https://web.archive.org/web/20240115294742id_/https://xyz.com/admin
```

Opening this URL attempts to retrieve the archived response associated with that timestamp.

**Important:** The `id_` modifier requests the archived content without the usual Wayback Machine toolbar and rewriting.

## 3. Discover Archived URLs Across Subdomains

Use the following CDX API query to discover historically archived URLs across a target domain and its subdomains.

```text
https://web.archive.org/cdx/search/cdx?url=*.xyz.com/*&collapse=urlkey&output=text&fl=original
```

**Parameters:**
- `url=*.xyz.com/*` – Searches for matching URLs across subdomains and paths.
- `collapse=urlkey` – Removes duplicate URL entries.
- `output=text` – Returns results in plain-text format.
- `fl=original` – Displays only original archived URLs.

**Use Case:** Identify historical API endpoints, forgotten subdomains, old admin panels, staging environments, and deprecated application routes.

**Note:** Results depend on URLs indexed by the Wayback Machine.

## 4. Practical Workflow

1. Identify the target domain.
2. Query the Wayback Machine CDX API.
3. Review the returned JSON records.
4. Identify interesting historical URLs.
5. Copy the corresponding timestamp.
6. Construct the archived URL using the `id_` modifier.
7. Review the retrieved historical content.

## 5. Security Research Applications

This technique can be useful during authorized reconnaissance and security assessments to discover:

- Previously exposed administrative pages.
- Historical API endpoints.
- Old login and authentication pages.
- Deprecated application routes.
- Previously accessible configuration files.
- Historical JavaScript and CSS resources.
- Old subdomain references.
- Potentially sensitive information that was publicly archived.

## 6. Important Notes

- An archived HTTP `200` response does not guarantee that the original resource is currently accessible.
- A `403` or `404` response in the CDX results reflects the archived response status, not necessarily the current website's status.
- Not every historical URL has an available snapshot.
- Archived content may be incomplete or contain broken resources.
- Do not assume that the absence of a URL from CDX means it never existed.

**Reference:** [Internet Archive – Wayback Machine](https://web.archive.org/)

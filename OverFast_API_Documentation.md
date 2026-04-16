# OverFast API Documentation

OverFast API provides free access to Overwatch 2 data including heroes, players, maps, and stats.
It pulls data directly from Blizzard's public pages and returns it in structured JSON format.

---

## Authentication

OverFast API requires NO API key and NO account registration.
All endpoints are freely and publicly accessible.

Simply send a GET request to any endpoint — no headers or credentials needed.

Rate Limiting Restrictions:
- If you send too many requests too quickly, you will receive a 429 error.
  You must wait 1 second before retrying (check the Retry-After response header).
- If Blizzard's servers are being scraped too frequently, you may receive a 503 error.
  You must wait 5 seconds before retrying (check the Retry-After response header).

---

## Base URL

https://overfast-api.tekrop.fr

---

## Endpoint 1 — Get a List of Heroes

GET /heroes

Returns a list of all Overwatch 2 heroes. You can filter by role or gamemode.
Cached for 1 day.

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| role      | Optional | Filter by hero role. Values: damage, support, tank |
| gamemode  | Optional | Filter by gamemode. Values: quickplay, stadium |
| locale    | Optional | Language for results. Default: en-us. Other options: fr-fr, de-de, es-es, ja-jp, ko-kr, pt-br, ru-ru, zh-tw |

### Sample Request

GET https://overfast-api.tekrop.fr/heroes?role=support&locale=en-us

### Sample Response

[
  {
    "key": "ana",
    "name": "Ana",
    "portrait": "https://d15f34w2p8l1cc.cloudfront.net/overwatch/3429c394716364bbef802180e9763d04812757c205e1b4568bc321772096ed86.png",
    "role": "support",
    "gamemodes": [
      "quickplay",
      "stadium"
    ]
  },
  {
    "key": "mercy",
    "name": "Mercy",
    "portrait": "https://d15f34w2p8l1cc.cloudfront.net/overwatch/64c4e1a5b35dc4c6e7e81e8f1a9f4d5b2c3a7e8d9f0b1c2d3e4f5a6b7c8d9e0f.png",
    "role": "support",
    "gamemodes": [
      "quickplay",
      "stadium"
    ]
  }
]

---

## Endpoint 2 — Get a Single Hero's Details

GET /heroes/{hero_key}

Returns full details for one hero including their description, abilities, hitpoints, age, location, and lore.
Cached for 1 day.

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| hero_key  | Required | The hero's unique ID (from the /heroes list). Example: ana, tracer, reinhardt, soldier-76 |
| locale    | Optional | Language for text fields. Default: en-us |

### Sample Request

GET https://overfast-api.tekrop.fr/heroes/ana

### Sample Response

{
  "name": "Ana",
  "description": "One of the founding members of Overwatch, Ana uses her skills and expertise to defend her homeland and protect the innocents caught in the crossfire of war.",
  "portrait": "https://d15f34w2p8l1cc.cloudfront.net/overwatch/3429c394716364bbef802180e9763d04812757c205e1b4568bc321772096ed86.png",
  "role": "support",
  "location": "Cairo, Egypt",
  "age": 60,
  "birthday": "23 Oct",
  "hitpoints": {
    "health": 200,
    "armor": 0,
    "shields": 0,
    "total": 200
  },
  "abilities": [
    {
      "name": "Biotic Rifle",
      "description": "Long-range rifle that heals allies and damages enemies with biotic rounds.",
      "icon": "https://d15f34w2p8l1cc.cloudfront.net/overwatch/55557d776b2ba5a8b684cbf4c99f5e0952da90ee36f5f1a05628d8e67dc8b940.png"
    },
    {
      "name": "Sleep Dart",
      "description": "Fires a dart that puts an enemy hero to sleep.",
      "icon": "https://d15f34w2p8l1cc.cloudfront.net/overwatch/0271210fc05e76a7e64b2fcae58f99c5f546e741c513a0c47264c8f7e055b827.png"
    },
    {
      "name": "Nano Boost",
      "description": "Ana enhances an ally, temporarily increasing their damage and reducing damage they take.",
      "icon": "https://d15f34w2p8l1cc.cloudfront.net/overwatch/e9b2e5a58c1f03f4f77f8b32a62baf3e5d6c7e8a9b0c1d2e3f4a5b6c7d8e9f0a.png"
    }
  ],
  "story": {
    "summary": "One of the founding members of Overwatch. After being presumed dead, Ana returned — older, wiser, and more determined than ever to protect the people she loves.",
    "chapters": [
      {
        "title": "Old Soldiers",
        "content": "Ana Amari dedicated her life to the protection of Overwatch. As one of its founding members and an exceptional soldier, she served as second-in-command during the Omnic Crisis and became one of the most decorated snipers in the world."
      }
    ]
  }
}

---

## Response Codes

| Code | Meaning | What To Do |
|------|---------|------------|
| 200  | Success | Read the JSON response normally |
| 404  | Hero not found | Check that the hero_key is spelled correctly |
| 429  | API rate limit reached | Wait 1 second, then retry |
| 500  | Internal server error | Try again later |
| 503  | Blizzard rate limit | Wait 5 seconds, then retry |
| 504  | Blizzard server error | Blizzard is down — try again later |

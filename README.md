# Robota.ua CV/Resume Database Scraper

Extract structured data from [robota.ua](https://robota.ua) — robota.ua candidate/CV database (CVDB) with 40+ filters including keywords, city, industry, experience, education, and salary. Returns structured career profiles with work history, skills, and languages for 5M+ Ukrainian job seekers. Incremental tracking.

**[Robota.ua CV/Resume Database Scraper on Apify →](https://apify.com/blackfalcondata/robota-ua-cv-scraper?fpr=1h3gvi)**

---

## Key features

**Incremental mode** — Only get new or changed listings since your last run. Content hash per listing — no duplicates, no re-processing.

**Change classification** — Track unchanged, expired, cross-run repost detection across runs. Build audit trails of how listings evolve over time.

**Compact output** — Emit core fields only (AI-agent / MCP-friendly). Keeps response size small for LLM workflows.

**Result cap** — Stop after N listings. Set to 0 for the full catalog.

**Export anywhere** — Download as JSON, CSV, or Excel. Stream via Apify API, webhooks, or integrations with Make, Zapier, Airbyte, Keboola.

**Structured data** — Every listing returns the same schema with consistent field naming. All fields always present — `null` when unavailable, never omitted.

---

## Use cases

**Data pipeline automation**
Integrate with your ETL pipeline to collect structured listings from robota.ua on a schedule. Export to CSV, JSON, or directly to your database. Use compact mode to control output size.

**Market research**
Monitor listings, track trends, and analyze market dynamics with structured, deduplicated data from robota.ua.

**Change monitoring**
Run daily or hourly in incremental mode to capture only new, updated, or expired listings. Perfect for price-tracking, churn analysis, and alerting pipelines.

**AI / LLM training data**
Structured JSON per listing is ready for RAG pipelines, embeddings, and agent workflows. Compact mode trims tokens for LLM context windows.

---

## Quick start

```json
{
  "keywords": "developer",
  "maxResults": 10
}
```

---

## Input parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `keywords` | string | — | Free-text search across CV fields (speciality, skills, experience descriptions). Leave empty to browse all CVs. |
| `cityId` | integer | `0` | Filter by candidate's city. 0 = all cities. Common IDs: 1=Kyiv, 2=Lviv, 3=Odesa, 4=Dnipro, 6=Donetsk, 9=Zaporizhzhia, 21=Kharkiv. Full list: GET https://api.robota.ua/dictionary/city |
| `rubricIds` | array | `[]` | Filter by industry rubrics from /values/rubrics. Example: [1] = IT. |
| `branchIds` | array | `[]` | Filter by business branch from /dictionary/branch. |
| `scheduleIds` | array | `[]` | Work schedule filter. 1=full-time, 2=part-time, 3=shift, 4=internship, 5=remote. |
| `experienceIds` | array | `[]` | 0=no experience, 1=up to 1yr, 2=1-2yr, 3=2-5yr, 4=5-10yr, 5=10+yr. |
| `educationIds` | array | `[]` | Education filter from /dictionary/education. |
| `languages` | array | `[]` | Language requirements from /values/languagelist. 1=English, 4=German, etc. |
| `ageFrom` | integer | — | Filter candidates at or above this age. |
| `ageTo` | integer | — | Filter candidates at or below this age. |
| `sex` | enum | `"Any"` | Gender filter. |
| `salaryFrom` | integer | — | Minimum salary expectation (in selected currency). |
| `salaryTo` | integer | — | Maximum salary expectation. |
| `currencyId` | integer | `1` | 1=UAH (default), 2=USD, 3=EUR. |
| `showCvWithoutSalary` | boolean | `true` | Include candidates who haven't declared a salary expectation. |
| `period` | enum | `"ThreeMonths"` | Only include CVs active within this period. |
| `sort` | enum | `"UpdateDate"` | How to sort results. |
| `searchType` | enum | `"WithSynonyms"` | Keyword matching strategy. |
| `ukrainian` | boolean | `true` | Search using Ukrainian language variant. |
| `inside` | boolean | `false` | When cityId is set, also include candidates from the surrounding oblast. |
| `moveability` | boolean | `true` | Include candidates from other cities willing to relocate. When used with cityId, results may include candidates not currently in that city. Set to false for strict city-only results. |
| `onlyMoveability` | boolean | `false` | Only candidates willing to relocate. |
| `hasPhoto` | boolean | `false` | Only include CVs with a profile photo. |
| `onlyStudents` | boolean | `false` | Only include student candidates. |
| `onlyDisabled` | boolean | `false` | Only include candidates with disability status. |
| `onlyVeterans` | boolean | `false` | Only include veteran candidates. |
| `maxResults` | integer | `100` | Maximum total CVs to return. 0 = unlimited. Critical: default queries match 190,000+ CVs. |
| `maxPages` | integer | `0` | Alternative cap by listing pages (20 CVs per page). 0 = unlimited, capped by maxResults. |
| `fetchDetail` | boolean | `true` | Fetch /resume/{id} for each CV to get experiences, educations, skills, languages. Disable for listing-only mode (faster, less data). |
| `excludeUploadedOnly` | boolean | `true` | Filter out CVs that only have an uploaded PDF (no structured data in JSON). Affects ~0.5% of results. |
| `minFillingPercentage` | integer | `0` | Only include CVs with at least N% completion (0-100). Higher = richer data quality. |
| `compact` | boolean | `false` | Strip heavy HTML description fields. Returns identity + summary only (for AI-agent/MCP workflows). |
| `incrementalMode` | boolean | `false` | Only emit new/updated CVs since last run. Requires stateKey. |
| `stateKey` | string | — | Stable identifier for incremental state. State is automatically scoped per search filter combination. |
| `emitUnchanged` | boolean | `false` | In incremental mode, also emit CVs that haven't changed (with changeType='unchanged'). |
| `emitExpired` | boolean | `false` | In incremental mode, also emit CVs that disappeared from results (with changeType='expired'). |
| `skipReposts` | boolean | `false` | Exclude listings detected as reposts of previously seen jobs. |

---

## Output fields

Every listing returns the same 60-field schema. Missing values are `null` — never omitted.

- `resumeId`
- `userId`
- `sourceUrl`
- `source`
- `firstName`
- `patronymic`
- `age`
- `birthDate`
- `sex`
- `photoUrl`
- `cityId`
- `cityName`
- `districtIds`
- `relocationCityIds`
- `speciality`
- `salaryExpected`
- `salaryCurrency`
- `salaryDisplay`
- `scheduleIds`
- `scheduleNames`
- `branchIds`
- `branchNames`
- `rubrics`
- `experienceSummary`
- `experiences`
- `experienceYears`
- `educations`
- `educationLevelId`
- `educationLevel`
- `skillsHtml`
- `skillsText`
- `additionals`
- `languages`
- `professionLevelId`
- `activityLevel`
- `skype`
- `diiaCertificate`
- `isAnonymous`
- `isOnline`
- `isVerified`
- `isStudent`
- `isDisabled`
- `isVeteran`
- `hasOwnCar`
- `hasFile`
- `hasPhone`
- `isHasTextSection`
- `fillingPercentage`
- `fillingTypeId`
- `fillingTypeName`
- `addedAt`
- `updatedAt`
- `lastActivityAt`
- `matchedKeywords`
- `matchedIn`
- `searchFingerprint`
- `contentQuality`
- `detailFetched`
- `scrapedAt`
- `changeType`

---

## Sample output

One object per listing. Here is a real example from a production run:

```json
{
  "resumeId": 21762019,
  "userId": 14433620,
  "sourceUrl": "https://robota.ua/ua/cv/21762019",
  "source": "robota.ua",
  "firstName": "Ірина",
  "patronymic": null,
  "age": null,
  "birthDate": "0001-01-01T00:00:00",
  "sex": 0,
  "photoUrl": "https://images.cf-rabota.com.ua/2017/04/non-photo.png",
  "cityId": 90,
  "cityName": "Павлоград"
}
```

*Truncated — full records contain 60 fields. See Output fields for the complete schema.*

**[Try Robota.ua CV/Resume Database Scraper now — $5 free credit, no credit card →](https://apify.com/blackfalcondata/robota-ua-cv-scraper?fpr=1h3gvi)**

---

## Pricing

Pay only for what you extract. No subscription required — Apify's free $5 credit covers thousands of results.

| Event | Price (USD) |
| --- | --- |
| Actor Start | $0.01 |
| Result | $0.002 |

See the [actor on Apify](https://apify.com/blackfalcondata/robota-ua-cv-scraper?fpr=1h3gvi) for current pricing.

---

## FAQ

**How do I scrape robota.ua?**
Use this actor on Apify to extract structured data from robota.ua. Configure your search query and filters in the input, then click Start — no coding required.

**How do I get robota.ua data as JSON, CSV, or Excel?**
The actor writes each listing to Apify's dataset. Download as JSON, CSV, or Excel from the Console, stream via the API, or push to Make, Zapier, Airbyte, or Keboola.

**Is it legal to scrape robota.ua?**
Web scraping of publicly available data is generally legal. This actor only accesses publicly visible information. Always check robota.ua's terms of service for your specific use case.

**How much does it cost?**
Pay-per-event pricing — you only pay for listings extracted. Apify's free $5 credit is enough to run thousands of results before you pay anything.

**How does incremental mode work?**
Each listing gets a content hash. On subsequent runs, only new or changed listings are emitted — saving time, compute, and storage. Expired listings can be tracked separately.

**Do I need an API key or credentials?**
No. Just sign up for Apify, paste your input, and click Start. No credit card required.

---

## Related products by Black Falcon Data

- [StepStone Scraper](https://apify.com/blackfalcondata/stepstone-scraper?fpr=1h3gvi) — Job listings from 18 European portals
- [Indeed Job Scraper](https://apify.com/blackfalcondata/indeed-job-scraper?fpr=1h3gvi) — Indeed job listings with salary data
- [Glassdoor Job Scraper](https://apify.com/blackfalcondata/glassdoor-job-scraper?fpr=1h3gvi) — Glassdoor listings with company ratings
- [Arbeitsagentur Scraper](https://apify.com/blackfalcondata/arbeitsagentur-scraper?fpr=1h3gvi) — Germany's official job portal (1M+ listings)
- [SEEK Scraper](https://apify.com/blackfalcondata/seek-scraper?fpr=1h3gvi) — Australia & NZ's largest job board
- [Naukri Scraper](https://apify.com/blackfalcondata/naukri-scraper?fpr=1h3gvi) — India's largest job portal

---

## Getting started with Apify

New to Apify? [Create a free account with $5 credit](https://console.apify.com/sign-up?fpr=1h3gvi) — no credit card required.

1. Sign up — $5 platform credit included
2. Open [Robota.ua CV/Resume Database Scraper](https://apify.com/blackfalcondata/robota-ua-cv-scraper?fpr=1h3gvi) and configure your input
3. Click **Start** — export results as JSON, CSV, or Excel

Need more later? [See Apify pricing](https://apify.com/pricing?fpr=1h3gvi).

---

## About Black Falcon Data

Black Falcon Data builds production-grade web scrapers for job boards and marketplace data. Browse our full actor catalog at [www.blackfalcondata.com](https://www.blackfalcondata.com).

---

*Last updated: 2026 04*

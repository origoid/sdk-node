# Reference

## Authentication

<details><summary><code>client.authentication.<a href="/src/api/resources/authentication/client/Client.ts">issueToken</a>({ ...params }) -> OrigoidApi.Envelope</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

**Credits:** Free — authentication endpoint.

Issues a short-lived Bearer JWT token from valid API Key or Basic credentials. The token can then be used as `Authorization: Bearer <token>` on subsequent requests instead of resending your long-lived API Key. Useful when you need to delegate access to a downstream client without sharing your primary credentials.

The token's lifetime is returned in the `expires_in` field (seconds). Tokens are stateless — there is no revocation endpoint; if compromised, rotate the underlying API Key instead.

</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.authentication.issueToken();
```

</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `OrigoidApi.IssueTokenRequest`

</dd>
</dl>

<dl>
<dd>

**requestOptions:** `Authentication.RequestOptions`

</dd>
</dl>
</dd>
</dl>

</dd>
</dl>
</details>

## RENAPO

<details><summary><code>client.renapo.<a href="/src/api/resources/renapo/client/Client.ts">validateCurp</a>({ ...params }) -> OrigoidApi.Envelope</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

**Credits:** 1 per call.

Validates a CURP (Clave Única de Registro de Población) against the official RENAPO registry and returns the full personal record associated with it: given names, surnames, gender, date of birth, birth state, document status (active, deceased, apocryphal, judicial suspension), and registration metadata.

Optionally generates the associated 13-character RFC (Registro Federal de Contribuyentes) when `generateRfc: true` is sent. RFC generation is deterministic from CURP and does not call SAT.

Use this endpoint when you have a CURP and need to confirm it is genuine, find out who owns it, or detect if the holder is deceased before extending a financial product.

</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.renapo.validateCurp({
    curp: "TEST900101HDFRRN09",
});
```

</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `OrigoidApi.ValidateCurpRequest`

</dd>
</dl>

<dl>
<dd>

**requestOptions:** `Renapo.RequestOptions`

</dd>
</dl>
</dd>
</dl>

</dd>
</dl>
</details>

<details><summary><code>client.renapo.<a href="/src/api/resources/renapo/client/Client.ts">lookupCurp</a>({ ...params }) -> OrigoidApi.Envelope</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

**Credits:** 2 per call.

Reconstructs a CURP from the four official input fields: given names, first surname, second surname, gender, date of birth, and birth state code. Calls RENAPO and returns the matching CURP plus the full personal record (same shape as `validateCurp`).

Use this endpoint when your KYC form collects names and date of birth but not the CURP, and you need the CURP to file a financial product or report to regulators. The lookup uses RENAPO's strict matching — if any field is misspelled, no match is returned (`CURP_NOT_FOUND`).

</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.renapo.lookupCurp({
    givenNames: "JUAN",
    firstSurname: "PEREZ",
    dateOfBirth: "1990-01-01",
    gender: "H",
    birthStateCode: "DF",
});
```

</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `OrigoidApi.LookupCurpRequest`

</dd>
</dl>

<dl>
<dd>

**requestOptions:** `Renapo.RequestOptions`

</dd>
</dl>
</dd>
</dl>

</dd>
</dl>
</details>

## IMSS

<details><summary><code>client.imss.<a href="/src/api/resources/imss/client/Client.ts">lookupNss</a>({ ...params }) -> OrigoidApi.Envelope</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

**Credits:** 1 per call.

Retrieves a worker's NSS (Número de Seguridad Social) from IMSS based on CURP. Returns the 11-digit NSS plus metadata.

Use this endpoint when onboarding employees for payroll or social-security registration: a CURP is far easier to collect than asking the candidate for their NSS card, which is frequently misplaced.

</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.imss.lookupNss({
    curp: "ALMR900805HDFRZA09",
});
```

</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `OrigoidApi.LookupNssRequest`

</dd>
</dl>

<dl>
<dd>

**requestOptions:** `Imss.RequestOptions`

</dd>
</dl>
</dd>
</dl>

</dd>
</dl>
</details>

<details><summary><code>client.imss.<a href="/src/api/resources/imss/client/Client.ts">getEmploymentStatus</a>({ ...params }) -> OrigoidApi.Envelope</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

**Credits:** 1 per call.

Returns the current IMSS employment status of a worker (identified by NSS or CURP): whether they are currently registered as employed (`ACTIVO`), inactive, the modality of registration, the registered employer's RFC, base salary, and date of last status change.

Use this endpoint for income verification (lending, leasing), employment confirmation (background checks), or to detect overlapping employment when complying with employment regulations.

</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.imss.getEmploymentStatus({
    curp: "GARM900101HDFRZA01",
    nss: "92038109713",
});
```

</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `OrigoidApi.GetEmploymentStatusRequest`

</dd>
</dl>

<dl>
<dd>

**requestOptions:** `Imss.RequestOptions`

</dd>
</dl>
</dd>
</dl>

</dd>
</dl>
</details>

## INE

<details><summary><code>client.ine.<a href="/src/api/resources/ine/client/Client.ts">validateVoterList</a>({ ...params }) -> OrigoidApi.Envelope</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

**Credits:** 1 per call.

Validates that a Mexican voter ID (INE / IFE) credential exists in INE's Lista Nominal — the official roll of registered voters — by sending CIC, OCR or ID number depending on the credential model. Returns a confirmation, the voter's polling section, and validity dates.

Use this endpoint as part of KYC to verify that the voter ID presented by your customer is registered and valid (not stolen, not lost, not cancelled).

</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.ine.validateVoterList({
    key: "value",
});
```

</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `OrigoidApi.ValidateVoterListRequest`

</dd>
</dl>

<dl>
<dd>

**requestOptions:** `Ine.RequestOptions`

</dd>
</dl>
</dd>
</dl>

</dd>
</dl>
</details>

<details><summary><code>client.ine.<a href="/src/api/resources/ine/client/Client.ts">extractVoterIdData</a>({ ...params }) -> OrigoidApi.Envelope</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

**Credits:** 1 per call.

Performs OCR on the front and back of a Mexican voter ID (INE / IFE) and returns the structured data printed on the credential: full name, CURP, voter key (CIC / OCR), address, photograph metadata, the document model variant (E, G, H), and the MRZ read from the back when present.

What sets this endpoint apart is **integrated address normalization + geocoding**: the address printed on the INE is rarely clean — abbreviations, missing colonia, inconsistent casing. We normalize and enrich it automatically. You get back not only the raw address text, but also:

- **`addressNormalized`**: corrected casing, expanded abbreviations (`AV.` → `AVENIDA`, `CALZ.` → `CALZADA`), validated postal code against the SEPOMEX directory, matched neighborhood / municipality / state from the official catalog, and `latitude` / `longitude` when the address resolves with confidence.
- **`electoralGeography`**: derived electoral district, federal entity, and polling section — useful for cross-checking with `validateVoterList`.
- **Document model detection** (E, G, H) and per-model security feature validation.
- **MRZ + QR cross-validation**: when the back contains MRZ and QR, we read both and confirm they agree with the printed fields. Mismatches are flagged.

Use this endpoint to digitize voter ID capture without manual transcription, and to obtain a geo-enriched address record in a single call — eliminating a separate geocoding step in your KYC flow.

</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.ine.extractVoterIdData({
    front: "front",
});
```

</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `OrigoidApi.ExtractVoterIdDataRequest`

</dd>
</dl>

<dl>
<dd>

**requestOptions:** `Ine.RequestOptions`

</dd>
</dl>
</dd>
</dl>

</dd>
</dl>
</details>

<details><summary><code>client.ine.<a href="/src/api/resources/ine/client/Client.ts">extractQrData</a>({ ...params }) -> OrigoidApi.Envelope</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

**Credits:** 2 per call.

Decrypts and parses the QR codes printed on Mexican voter IDs (INE models G and H). The two QRs on the back contain RSA-signed payloads with the holder's full record (name, CURP, voter key, address, signature). This endpoint decrypts both QRs and merges the result.

Use this endpoint as a tamper-evidence check: if the QR decrypts successfully and matches the printed data, the credential is highly likely to be authentic.

</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.ine.extractQrData({
    back: "back",
});
```

</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `OrigoidApi.ExtractQrDataRequest`

</dd>
</dl>

<dl>
<dd>

**requestOptions:** `Ine.RequestOptions`

</dd>
</dl>
</dd>
</dl>

</dd>
</dl>
</details>

## Biometrics

<details><summary><code>client.biometrics.<a href="/src/api/resources/biometrics/client/Client.ts">matchFaces</a>({ ...params }) -> OrigoidApi.Envelope</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

**Credits:** 1 per call.

Compares two facial images and returns a similarity score (0–100) plus a binary match/no-match decision. Typical use is 1:1 verification between a live selfie and the photograph on an ID document.

Use this endpoint to confirm that the person presenting an ID is the same person depicted on it. Combine with `checkLiveness` to also defend against presentation attacks (photo of a photo, printed mask).

</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.biometrics.matchFaces({
    face: "face",
    front: "front",
});
```

</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `OrigoidApi.MatchFacesRequest`

</dd>
</dl>

<dl>
<dd>

**requestOptions:** `Biometrics.RequestOptions`

</dd>
</dl>
</dd>
</dl>

</dd>
</dl>
</details>

<details><summary><code>client.biometrics.<a href="/src/api/resources/biometrics/client/Client.ts">checkLiveness</a>({ ...params }) -> OrigoidApi.Envelope</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

**Credits:** 1 per call.

Analyzes a selfie to determine whether it depicts a real, live person in front of the camera (`isLive: true`) or a spoofing attempt (printed photo, screen replay, mask). Returns a liveness score, confidence level, and detected attack types when applicable.

Use this endpoint at the start of a remote KYC flow to filter out automated bots, recycled images, and basic presentation attacks before invoking heavier downstream checks.

</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.biometrics.checkLiveness({
    selfie: "selfie",
});
```

</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `OrigoidApi.CheckLivenessRequest`

</dd>
</dl>

<dl>
<dd>

**requestOptions:** `Biometrics.RequestOptions`

</dd>
</dl>
</dd>
</dl>

</dd>
</dl>
</details>

## Compliance

<details><summary><code>client.compliance.<a href="/src/api/resources/compliance/client/Client.ts">searchSat69</a>({ ...params }) -> OrigoidApi.Envelope</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

**Credits:** 1 per call.

Validates if an individual or legal entity is listed in the Mexican Tax Authority (SAT) Article 69 blacklist. This endpoint covers all sub-lists of Art. 69. Note: This endpoint does NOT evaluate Article 69-B (EFOS/simulated operations).

**Business Rules (Search Priority):**

1. The client must provide EITHER a name (`name` / Razón Social) OR an exact identifier (`rfc`).
2. If `rfc` is provided, the backend performs a strict exact match. If only `name` is provided, the backend performs a highly restrictive text search.

**Risk Level Matrix (`riskLevel`):**

- `NONE`: No matches found. Safe for automated approval.
- `LOW` (Informativo / Sin Riesgo Operativo): The subject has historical or administrative records but is legally operating. **Lists:** Condonados (Todos los decretos/artículos), Reducción Art. 74 CFF, Retorno de Inversiones, Entes Públicos y de Gobierno Omisos.
- `MEDIUM` (Riesgo Financiero / Morosidad): The subject has active enforceable debts or the SAT declared them insolvent/uncollectible. **Lists:** Firmes, Exigibles, Cancelados (Incosteabilidad / Insolvencia).
- `HIGH` (Riesgo Operativo Grave): The subject cannot be found by authorities or their digital billing seals (CSD) have been revoked, halting their operations. **Lists:** No Localizados, CSD Sin Efectos.
- `CRITICAL` (Riesgo Legal / Fraude Penal): The subject has criminal convictions related to tax crimes. **Lists:** Sentencias.
  </dd>
  </dl>
  </dd>
  </dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.compliance.searchSat69({
    name: "JUAN PEREZ LOPEZ",
});
```

</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `OrigoidApi.SearchSat69Request`

</dd>
</dl>

<dl>
<dd>

**requestOptions:** `Compliance.RequestOptions`

</dd>
</dl>
</dd>
</dl>

</dd>
</dl>
</details>

<details><summary><code>client.compliance.<a href="/src/api/resources/compliance/client/Client.ts">searchSat69B</a>({ ...params }) -> OrigoidApi.Envelope</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

**Credits:** 1 per call.

Validates if an individual or legal entity is listed in the Mexican Tax Authority (SAT) Article 69-B blacklist. This list is specifically for EFOS (Empresas que Facturan Operaciones Simuladas), commonly known as 'Factureros' or shell companies involved in tax fraud and money laundering.

**Business Rules (Search Priority):**

1. The client must provide EITHER a name (`name` / Razón Social) OR an exact identifier (`rfc`).
2. If `rfc` is provided, the backend performs a strict exact match. If only `name` is provided, the backend performs a highly restrictive text search.

**Risk Level Matrix (`riskLevel` mapped to SAT Status):**

- `NONE`: No matches found in the SAT 69-B list. Safe for automated approval.
- `LOW`: The SAT status is **'Desvirtuado'** (Investigated but successfully proved innocence) or **'Sentencia Favorable'** (Won in court / cleared). Provided for audit trails and historical record.
- `MEDIUM`: Reserved for intermediate risk states. Currently not produced by the SAT 69-B classification.
- `HIGH`: The SAT status is **'Presunto'** (Currently under investigation for simulated operations). Extreme caution advised; usually triggers Enhanced Due Diligence (EDD) or temporal blocks.
- `CRITICAL`: The SAT status is **'Definitivo'** (Confirmed shell company / EFOS). Legally binding block required for AML compliance.
  </dd>
  </dl>
  </dd>
  </dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.compliance.searchSat69B({
    name: "JUAN PEREZ LOPEZ",
});
```

</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `OrigoidApi.SearchSat69BRequest`

</dd>
</dl>

<dl>
<dd>

**requestOptions:** `Compliance.RequestOptions`

</dd>
</dl>
</dd>
</dl>

</dd>
</dl>
</details>

<details><summary><code>client.compliance.<a href="/src/api/resources/compliance/client/Client.ts">searchOfac</a>({ ...params }) -> OrigoidApi.Envelope</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

**Credits:** 1 per call.

Searches the official OFAC sanctions lists (SDN, Non-SDN, FSE, NS-ISA, SSI, CAPTA, NS-PLC) for the provided name or identifier. Returns matches with the originating list, sanction programs (CUBA, IRAN, RUSSIA, etc.), entity type (individual, entity, vessel, aircraft), and risk level.

Use this endpoint as part of mandatory AML compliance to detect counterparties subject to United States sanctions before extending financial services. Required by CNBV for regulated financial institutions and recommended for any cross-border activity.

</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.compliance.searchOfac({
    name: "JOAQUIN GUZMAN LOERA",
});
```

</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `OrigoidApi.SearchOfacRequest`

</dd>
</dl>

<dl>
<dd>

**requestOptions:** `Compliance.RequestOptions`

</dd>
</dl>
</dd>
</dl>

</dd>
</dl>
</details>

<details><summary><code>client.compliance.<a href="/src/api/resources/compliance/client/Client.ts">searchPeps</a>({ ...params }) -> OrigoidApi.Envelope</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

**Credits:** 1 per call.

Searches for the subject in our consolidated PEP (Politically Exposed Persons) database — including active PEPs, former PEPs (EX_PEP), and their immediate family and close associates (PEP_AFFINITY, EX_PEP_AFFINITY). Accepts full name, CURP, or RFC for matching.

Returns each match with the political position held, institution, status (active/inactive), country, and risk level. Use this endpoint as part of enhanced due diligence for clients in regulated financial products (LFPIORPI requirements).

</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.compliance.searchPeps({
    givenNames: "JUAN",
    firstSurname: "PEREZ",
    secondSurname: "LOPEZ",
});
```

</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `OrigoidApi.SearchPepsRequest`

</dd>
</dl>

<dl>
<dd>

**requestOptions:** `Compliance.RequestOptions`

</dd>
</dl>
</dd>
</dl>

</dd>
</dl>
</details>

## Email

<details><summary><code>client.email.<a href="/src/api/resources/email/client/Client.ts">validateEmail</a>({ ...params }) -> OrigoidApi.Envelope</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

**Credits:** 1 per call.

Validates an email address for deliverability and risk. Returns the normalized address, deliverability verdict (`deliverable`, `risky`, `undeliverable`), a quality score (0–100), a toxicity score, and a set of boolean verdicts (is_free, is_disposable, is_role_account, is_full_mailbox, is_catch_all, is_toxic) plus DNS/SMTP infrastructure metadata.

Use this endpoint at signup time to reject typos and disposable addresses before they enter your database, reducing bounce rates on transactional email and fraud signals from throwaway accounts.

</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.email.validateEmail({
    email: "user@example.com",
});
```

</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `OrigoidApi.ValidateEmailRequest`

</dd>
</dl>

<dl>
<dd>

**requestOptions:** `Email.RequestOptions`

</dd>
</dl>
</dd>
</dl>

</dd>
</dl>
</details>

## Proof of Address

<details><summary><code>client.proofOfAddress.<a href="/src/api/resources/proofOfAddress/client/Client.ts">extractProofOfAddress</a>({ ...params }) -> OrigoidApi.Envelope</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

**Credits:** 1 per call.

Performs OCR on a Mexican proof-of-address document — utility bills (water, electricity, gas, internet, telephone) and bank statements — and returns the structured data printed on it.

Returns:

- **`provider`**: the issuing utility or institution (CFE, Telmex, Agua, etc.), so you can apply provider-specific business rules and recognize legitimate document layouts.
- **`personalInfo`**: holder name as printed on the document.
- **`address`**: full address as printed (street, exterior / interior number, neighborhood, municipality, state, postal code).
- **`billing`**: issuance date, account number, period covered.
- **`validations`**: flags about document age, document type detection confidence, and structural consistency checks.

Use this endpoint to automate address verification in KYC flows. The extracted address can be cross-checked against the address your customer submitted at signup.

</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.proofOfAddress.extractProofOfAddress({
    file: "file",
});
```

</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `OrigoidApi.ExtractProofOfAddressRequest`

</dd>
</dl>

<dl>
<dd>

**requestOptions:** `ProofOfAddress.RequestOptions`

</dd>
</dl>
</dd>
</dl>

</dd>
</dl>
</details>

## SAT

<details><summary><code>client.sat.<a href="/src/api/resources/sat/client/Client.ts">validateRfc</a>({ ...params }) -> OrigoidApi.Envelope</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

**Credits:** 1 per call.

Validates the structure and current status of a Mexican RFC (Registro Federal de Contribuyentes) against the SAT registry. Returns the taxpayer type (individual or legal entity), full registered name, fiscal regime, and registration status.

Use this endpoint to confirm that the RFC your customer provided is real, well-formed, and currently active with SAT before extending credit, issuing invoices, or signing contracts.

</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.sat.validateRfc({
    rfc: "GARM900101HDF",
});
```

</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `OrigoidApi.ValidateRfcRequest`

</dd>
</dl>

<dl>
<dd>

**requestOptions:** `Sat.RequestOptions`

</dd>
</dl>
</dd>
</dl>

</dd>
</dl>
</details>

<details><summary><code>client.sat.<a href="/src/api/resources/sat/client/Client.ts">extractCsf</a>({ ...params }) -> OrigoidApi.Envelope</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

**Credits:** 1 per call.

Extracts structured data from a Constancia de Situación Fiscal (CSF) — the official PDF document issued by SAT that proves a taxpayer's fiscal situation. You can submit the CSF as a base64-encoded file (PDF/PNG/JPG) and get back the full content as JSON, or alternatively pass RFC + CIF (the tax-certificate code) to retrieve the same data directly from SAT's public QR validator.

Returns the legal name, address, fiscal regime, economic activities, registration date, and tax obligations. Use this endpoint to automate vendor onboarding and to keep your records of partners' fiscal data continuously up to date.

</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.sat.extractCsf({
    rfc: "PELJ900101AAA",
    cif: "12345678901",
});
```

</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `OrigoidApi.ExtractCsfRequest`

</dd>
</dl>

<dl>
<dd>

**requestOptions:** `Sat.RequestOptions`

</dd>
</dl>
</dd>
</dl>

</dd>
</dl>
</details>

<details><summary><code>client.sat.<a href="/src/api/resources/sat/client/Client.ts">validateCfdi</a>({ ...params }) -> OrigoidApi.Envelope</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

**Credits:** 1 per call.

Validates a CFDI (Comprobante Fiscal Digital por Internet) — Mexico's mandatory electronic invoice — by checking its current status with SAT. Returns whether the CFDI is currently valid (`VALID`) or cancelled (`CANCELED`), the cancellation status (e.g. requires receiver acceptance), and the fiscal effect (`INCOME`, `EXPENSE`, `TRANSPORT`, `PAYROLL`, `PAYMENT`).

Use this endpoint when reconciling supplier invoices, processing expense reports, or ensuring that the invoices you receive are real and not later cancelled by the issuer without your knowledge.

</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.sat.validateCfdi({
    key: "value",
});
```

</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `OrigoidApi.ValidateCfdiRequest`

</dd>
</dl>

<dl>
<dd>

**requestOptions:** `Sat.RequestOptions`

</dd>
</dl>
</dd>
</dl>

</dd>
</dl>
</details>

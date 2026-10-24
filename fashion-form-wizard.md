# Fashion Industry Form Wizard

A responsive, accessible multi-step form wizard for fashion brands, designers, retailers, and creative teams to submit a new collection or product launch brief.

## Overview

**Goal:** Help users create a structured fashion collection brief through a clear, guided workflow instead of a long single-page form.

**Primary users:** Fashion designers, independent labels, brand managers, merchandisers, and fashion e-commerce teams.

**Example use case:** A fashion label prepares its Spring/Summer collection launch and submits brand, collection, product, production, and marketing details.

## Wizard steps

| Step | Name | Fields | Validation |
| --- | --- | --- | --- |
| 1 | Brand Profile | Brand name, contact email, brand category, target market, website | Brand name and valid email required |
| 2 | Collection Details | Collection name, season, year, inspiration, style direction, target audience | Collection name, season, and year required |
| 3 | Products & Sizing | Product categories, number of styles, size range, materials, color palette, price range | At least one category, valid size range, and price range required |
| 4 | Production & Sustainability | Production model, manufacturing location, estimated quantity, launch date, sustainability practices | Quantity must be positive; launch date required |
| 5 | Marketing & Assets | Sales channels, campaign goals, image uploads, social links, additional notes | Validate file type and size when files are provided |
| 6 | Review & Submit | Read-only summary, edit links, consent checkbox | Consent required before submission |

## Detailed field specification

### Step 1 — Brand Profile

- `brandName`: text, required, 2–100 characters
- `contactEmail`: email, required
- `brandCategory`: select (`Luxury`, `Contemporary`, `Streetwear`, `Activewear`, `Sustainable`, `Other`)
- `targetMarket`: multi-select (regional or international markets)
- `website`: URL, optional

### Step 2 — Collection Details

- `collectionName`: text, required
- `season`: select (`Spring/Summer`, `Fall/Winter`, `Resort`, `Pre-Fall`, `Seasonless`), required
- `year`: number, required
- `inspiration`: textarea, optional, max 1,000 characters
- `styleDirection`: multi-select (`Minimal`, `Classic`, `Avant-garde`, `Casual`, `Formal`, `Sporty`, `Other`)
- `targetAudience`: select (`Women`, `Men`, `Unisex`, `Kids`, `All`)

### Step 3 — Products & Sizing

- `categories`: checkbox group (`Tops`, `Bottoms`, `Dresses`, `Outerwear`, `Footwear`, `Accessories`), required
- `styleCount`: integer, min 1
- `sizes`: multi-select (`XXS`, `XS`, `S`, `M`, `L`, `XL`, `XXL`, `Custom`), required
- `materials`: repeatable text entries
- `colors`: repeatable color values with accessible text labels
- `priceMin` / `priceMax`: nonnegative numbers; `priceMax >= priceMin`
- `currency`: select (`USD`, `EUR`, `GBP`, `AZN`, `Other`)

### Step 4 — Production & Sustainability

- `productionModel`: radio (`In-house`, `Outsourced`, `Made-to-order`, `Hybrid`)
- `manufacturingLocation`: text
- `estimatedUnits`: positive integer
- `launchDate`: date, required
- `sustainabilityPractices`: checkbox group (`Recycled materials`, `Organic materials`, `Local production`, `Low-waste pattern cutting`, `Traceable supply chain`, `None`, `Other`)

### Step 5 — Marketing & Assets

- `salesChannels`: checkbox group (`Brand website`, `Marketplace`, `Physical retail`, `Wholesale`, `Pop-up`)
- `campaignGoals`: textarea
- `lookbookFiles`: optional multiple upload (JPG, PNG, WEBP, PDF; configurable max 10 MB each)
- `socialLinks`: optional list of URLs
- `notes`: optional textarea

### Step 6 — Review & Submit

Display a grouped summary of every step with **Edit** links. Include an explicit consent checkbox for submitting the supplied business information. Submit only after client-side and server-side validation succeeds.

## UX and interaction requirements

1. Display a stepper with current step, completed steps, and a text label such as `Step 2 of 6`.
2. Provide **Back**, **Continue**, **Save draft**, and final **Submit brief** actions as appropriate.
3. Validate the current step when the user selects Continue. Show field-level errors with actionable messages.
4. Preserve values when navigating backward or forward.
5. Allow users to return to completed steps from the stepper without losing data.
6. Autosave drafts locally when appropriate; for authenticated users, prefer secure server-side drafts. Explain storage behavior to the user.
7. Warn before discarding an unsaved draft.
8. Use responsive single-column layouts on mobile and a restrained two-column field layout on wider screens.
9. Ensure keyboard navigation, visible focus states, proper labels, error associations, and screen-reader announcements.
10. Respect reduced-motion preferences. Do not rely on color alone for progress or errors.
11. Display a loading state during submission and prevent duplicate submissions.
12. After success, show a confirmation screen with a reference ID and an option to start a new brief.

## Suggested visual direction

- **Style:** Editorial, minimal, premium fashion aesthetic
- **Palette:** Warm ivory backgrounds, charcoal text, subtle neutral borders, one restrained accent color
- **Typography:** High-contrast display serif for headings paired with a legible sans-serif for UI labels and body text
- **Spacing:** Generous whitespace, consistent 8px spacing scale
- **Components:** Stepper, input, select, radio group, checkbox group, tag selector, color swatch, upload zone, summary card, toast, buttons

## Example data model

```json
{
  "brand": {
    "brandName": "Atelier Example",
    "contactEmail": "studio@example.com",
    "brandCategory": "Contemporary",
    "targetMarket": ["Europe"],
    "website": "https://example.com"
  },
  "collection": {
    "collectionName": "Modern Forms",
    "season": "Spring/Summer",
    "year": 2027,
    "inspiration": "Modern architecture and natural textures",
    "styleDirection": ["Minimal", "Classic"],
    "targetAudience": "Unisex"
  },
  "products": {
    "categories": ["Tops", "Outerwear"],
    "styleCount": 12,
    "sizes": ["S", "M", "L"],
    "materials": ["Organic cotton", "Linen"],
    "colors": ["#E8E0D5", "#292929"],
    "priceMin": 75,
    "priceMax": 350,
    "currency": "USD"
  },
  "production": {
    "productionModel": "Made-to-order",
    "manufacturingLocation": "Local studio",
    "estimatedUnits": 250,
    "launchDate": "2027-03-15",
    "sustainabilityPractices": ["Organic materials", "Local production"]
  },
  "marketing": {
    "salesChannels": ["Brand website", "Pop-up"],
    "campaignGoals": "Introduce the new collection",
    "lookbookFiles": [],
    "socialLinks": [],
    "notes": ""
  },
  "consent": true
}
```

## Suggested implementation

- **Frontend:** React + TypeScript (Next.js optional)
- **Forms:** React Hook Form
- **Validation:** Zod, with schemas per step and a complete submission schema
- **Styling:** Tailwind CSS or CSS modules
- **Persistence:** Versioned local draft storage or authenticated backend drafts
- **Uploads:** Secure object storage with file type/size validation on the server
- **Submission:** API endpoint with server-side validation, rate limiting, and idempotency handling

### Suggested project structure

```text
src/
  components/
    wizard/
      FormWizard.tsx
      Stepper.tsx
      WizardNavigation.tsx
    fields/
      FileUpload.tsx
      ColorPicker.tsx
  features/
    fashion-brief/
      steps/
        BrandProfile.tsx
        CollectionDetails.tsx
        ProductsSizing.tsx
        Production.tsx
        MarketingAssets.tsx
        ReviewSubmit.tsx
      schema.ts
      types.ts
      defaults.ts
  app/
    fashion-brief/
      page.tsx
```

## Acceptance criteria

- [ ] All six steps are implemented and usable on mobile and desktop.
- [ ] Required fields and cross-field constraints are validated.
- [ ] Users can navigate backward without losing entered values.
- [ ] Drafts can be saved and restored as described in the UI.
- [ ] Progress and errors are accessible to keyboard and assistive-technology users.
- [ ] The review screen accurately reflects all entered information.
- [ ] Submission is validated on the server and cannot accidentally create duplicate records.
- [ ] File uploads are safely restricted and validated.
- [ ] Successful submissions display a confirmation and reference ID.

## GitHub usage

Add this file to your repository as `docs/fashion-form-wizard.md` and link it from the project `README.md`. It can serve as a product requirements document, implementation checklist, and shared reference for design and development.

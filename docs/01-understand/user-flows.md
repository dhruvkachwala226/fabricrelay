# User flows

## Existing manual workflow hypothesis

1. A buyer receives fabric photos, a forwarded PDF, or a physical catalog, or messages a wholesaler with a sourcing brief.
2. Staff recover the fabric identity from a photo, design code or sample book and check available colors, current rates and MOQ in chats, a stock sheet or with another staff member.
3. The buyer asks for missing details and decides whether to inspect a physical sample or request a bulk quote.
4. Staff record or forward the request, clarify the desired color or quantity, and follow up in chat or by phone. They may also note the request in a separate file.
5. Staff update their source and resend affected material when pricing or availability changes. Old copies may remain in circulation.

This sequence is for field testing. See the [Surat interview guide](market-validation.md).

## Proposed FabricRelay workflow

An authorized wholesaler maintains one branded catalog. Buyers open a shareable link without an account and browse published fabrics. Sample and wholesale quotation submissions appear in the same tenant’s queue. Staff respond outside FabricRelay and update status in the queue.

### 1. Super Admin management

```mermaid
flowchart TD
    A["SUPER_ADMIN signs in"] --> B["View businesses and recent platform activity"]
    B --> C{"Choose action"}
    C -->|Create or edit| D["Save business profile and account details"]
    C -->|Activate or suspend| E["Confirm status change"]
    D --> F["Record actor, business, action and time"]
    E --> F
    F --> B
```

### 2. Wholesaler onboarding

```mermaid
flowchart TD
    A["SUPER_ADMIN provisions a business and assigns an admin"] --> B["Admin completes invitation"]
    B --> C{"Active business and authorized membership?"}
    C -->|No| D["Deny management access"]
    C -->|Yes| E["WHOLESALER_ADMIN saves branding"]
    E --> F["Open catalog workspace"]
```

### 3. Fabric catalog creation

```mermaid
flowchart TD
    A["Start a fabric draft"] --> B["Upload photo and add specs and design code"]
    B --> C["Add named colors, availability, price visibility and MOQ"]
    C --> D{"Ready to publish?"}
    D -->|No| E["Save draft or correct errors"]
    E --> B
    D -->|Yes| F["Publish fabric in branded catalog"]
```

### 4. Catalog sharing

```mermaid
flowchart TD
    A["WHOLESALER_ADMIN opens published catalog"] --> B["Copy stable catalog URL"]
    B --> C["Paste into a WhatsApp message or other channel"]
    C --> D["Buyer opens the link without signing in"]
    D --> E{"Catalog public and business active?"}
    E -->|Yes| F["Show tenant's published catalog"]
    E -->|No| G["Show unavailable state"]
```

### 5. Buyer fabric discovery

```mermaid
flowchart TD
    A["Buyer opens catalog"] --> B["Browse or search by quality or design code"]
    B --> C["Filter by color or availability"]
    C --> D{"Published matches?"}
    D -->|No| E["Show empty state and reset filters"]
    E --> B
    D -->|Yes| F["Open fabric and review specs, color, rate visibility and MOQ"]
    F --> G["Choose sample or quotation request"]
```

### 6. Sample request submission

```mermaid
flowchart TD
    A["Open sample form on a fabric"] --> B["Select color and provide name and contact"]
    B --> C{"Input valid and catalog available?"}
    C -->|No| D["Show errors or unavailable state"]
    D --> B
    C -->|Yes| E["Save sample inquiry for resolved tenant"]
    E --> F["Show confirmation and request reference"]
```

### 7. Wholesale quotation submission

```mermaid
flowchart TD
    A["Open quotation form on a fabric"] --> B["Select color and enter planned meters, name and contact"]
    B --> C{"Input valid and catalog available?"}
    C -->|No| D["Show errors or unavailable state"]
    D --> B
    C -->|Yes| E["Flag quantity below listed MOQ when relevant"]
    E --> F["Save quote inquiry for resolved tenant"]
    F --> G["Show confirmation and request reference"]
```

### 8. Inquiry management

```mermaid
flowchart TD
    A["WHOLESALER_ADMIN opens tenant inquiry queue"] --> B["View new sample and quotation requests"]
    B --> C["Open request with fabric, color and contact context"]
    C --> D["Contact buyer through an existing channel"]
    D --> E["Mark In progress or Closed"]
    E --> B
```

See the [requirements](requirements.md) for the acceptance criteria and [MVP scope](mvp-scope.md) for deferred features.

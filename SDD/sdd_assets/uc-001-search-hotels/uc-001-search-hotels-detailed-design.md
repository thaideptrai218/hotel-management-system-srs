# 3.1 UC-001 - Search Hotels

## 3.1.1 Design Purpose

This section describes the detailed design for **UC-001 Search Hotels**. The use case allows Guest and Customer actors to search approved hotels by destination, stay dates, guest count, and optional filters. The design is based on the SRS only; class names and methods are conceptual design assumptions and must be revalidated against the final codebase.

**Related SRS items:** FEAT-MKT, UC-001, SCR-004, SCR-005, ENT-005, ENT-007, ENT-008, ENT-010, ENT-011, ENT-012, BR-MKT-001, BR-BOOK-001, BR-ROOM-002, TR-001, AT-UC001-04A, AT-UC001-05A.

The search flow must:

- Accept destination, check-in, check-out, guest count, filters, and search button input from SCR-004.
- Enforce BR-BOOK-001: Check-out date must be later than check-in date.
- Return only approved, active, and publicly available hotels under BR-MKT-001.
- Exclude blocked, inactive, occupied, dirty, cleaning, inspection-required, maintenance, or out-of-service rooms under BR-ROOM-002.
- Display SCR-005 result criteria summary, filters, hotel cards, empty state, and select hotel navigation.
- Ensure no booking data is created by failed validation or no-result paths.

## 3.1.2 Class Diagram

This part presents the class diagram for UC-001 Search Hotels.

![DGM-CLS-UC-001 - Search Hotels Class Diagram](./dgm-cls-uc-001-search-hotels-class-diagram.png)

**Figure 3.1-1: Class Diagram of UC-001 Search Hotels**

## 3.1.3 Class Specifications

This part explains the key methods shown in the class diagram. The classes are conceptual design assumptions because no implementation codebase was inspected.

### MarketplaceController Class

**Description:** API entry point for public marketplace search requests.

| No | Method | Description |
|---:|---|---|
| 1 | `searchHotels(request)` | Receives search criteria, coordinates validation, calls the search service, and returns `HotelSearchResponse`. |
| 2 | `validateSearchRequest(request)` | Checks required destination/date/guest fields and optional filters before service execution. |

### HotelSearchRequest Class

**Description:** Request DTO carrying criteria from SCR-004 to the backend.

| No | Method | Description |
|---:|---|---|
| 1 | `hasValidDateRange()` | Confirms check-out date is later than check-in date. |
| 2 | `hasValidGuestCount()` | Confirms guest count is positive for capacity matching. |

### HotelSearchService Class

**Description:** Application service that coordinates marketplace visibility, availability, filtering, and response assembly.

| No | Method | Description |
|---:|---|---|
| 1 | `searchApprovedHotels(request)` | Executes UC-001 main flow for approved public hotels with available room types. |
| 2 | `applyFilters(hotels, request)` | Applies price, amenity, and availability filters. |
| 3 | `buildSearchResponse(results)` | Builds result cards, criteria summary, and empty-state metadata. |

### AvailabilityQueryService Class

**Description:** Service that calculates room-type availability for the requested date range and guest count.

| No | Method | Description |
|---:|---|---|
| 1 | `findAvailableRoomTypes(hotelIds, dateRange, guestCount)` | Finds room types that satisfy capacity and available quantity. |
| 2 | `countAvailableQuantity(roomTypeId, dateRange)` | Counts available quantity while excluding non-assignable physical room statuses. |

### HotelSearchResponse Class

**Description:** Response DTO returned to SCR-005 with hotel results and display metadata.

| No | Method | Description |
|---:|---|---|
| 1 | `includeResultSummary()` | Adds total count, criteria summary, empty state, and result card metadata. |

### HotelRepository Class

**Description:** Repository for retrieving marketplace-visible hotel property data.

| No | Method | Description |
|---:|---|---|
| 1 | `findApprovedActivePublicByDestination(destination)` | Finds hotels that match destination and satisfy BR-MKT-001. |
| 2 | `findWithAmenities(hotelIds)` | Loads amenity data for search cards and filters. |

### RoomTypeRepository Class

**Description:** Repository for active private room types that can satisfy guest count.

| No | Method | Description |
|---:|---|---|
| 1 | `findActiveRoomTypes(hotelIds)` | Retrieves active private room types for candidate hotels. |
| 2 | `findRoomTypesMatchingCapacity(guestCount)` | Finds room types whose capacity satisfies guest count. |

### RoomAvailabilityRepository Class

**Description:** Repository for availability/block records by room type and date range.

| No | Method | Description |
|---:|---|---|
| 1 | `findAvailabilityByRoomTypes(roomTypeIds, dateRange)` | Loads availability records that overlap the requested stay. |
| 2 | `countBlockedQuantity(roomTypeId, dateRange)` | Counts blocked or unavailable quantity for a room type. |

### HotelProperty Class

**Description:** Domain entity representing a hotel listed on the marketplace.

| No | Method | Description |
|---:|---|---|
| 1 | `isApprovedActiveAndPublic()` | Returns whether the hotel can appear in search/detail pages. |

### RoomType Class

**Description:** Domain entity representing a private room type offered by a hotel.

| No | Method | Description |
|---:|---|---|
| 1 | `canFitGuestCount(guestCount)` | Checks whether capacity can satisfy guest count. |
| 2 | `isActive()` | Returns whether the room type can be displayed in marketplace search. |

### RoomAvailability Class

**Description:** Domain entity representing availability or block status over a date range.

| No | Method | Description |
|---:|---|---|
| 1 | `overlaps(dateRange)` | Checks whether the record overlaps the requested stay. |
| 2 | `isAvailable()` | Returns whether the record allows marketplace availability. |

## 3.1.4 Sequence Diagram

This part presents the sequence diagrams for UC-001 Search Hotels. The main-flow diagram shows only the successful scenario. Each alternative/error scenario has its own diagram.

![DGM-SEQ-UC-001 - Search Hotels Main Flow](./dgm-seq-uc-001-search-hotels-main-flow.png)

**Figure 3.1-2: Sequence Diagram of UC-001 Search Hotels - Main Flow**

### AT-UC001-04A - Invalid Date Range

- **Branch from Main Step:** 4
- **Condition:** Invalid date range.
- **Expected Response:** Check-out date must be later than check-in date. The system keeps submitted data unchanged and allows correction.

![DGM-SEQ-UC-001 - Invalid Date Range](./dgm-seq-uc-001-search-hotels-at-uc001-04a-invalid-date-range.png)

**Figure 3.1-3: Sequence Diagram of UC-001 Search Hotels - AT-UC001-04A Invalid Date Range**

### AT-UC001-05A - No Matching Hotels

- **Branch from Main Step:** 5
- **Condition:** No matching hotels.
- **Expected Response:** No hotels match your search criteria. Please adjust your search. No booking data is created.

![DGM-SEQ-UC-001 - No Matching Hotels](./dgm-seq-uc-001-search-hotels-at-uc001-05a-no-matching-hotels.png)

**Figure 3.1-4: Sequence Diagram of UC-001 Search Hotels - AT-UC001-05A No Matching Hotels**

### Validation, Authorization, Transaction, and Error Handling Notes

| Area | Design |
|---|---|
| Validation | Validate destination, check-in, check-out, guest count, and filters. Check-out date must be later than check-in date. |
| Authorization | UC-001 is public. Guest and Customer can search, but management-only data is not exposed. |
| Transaction | Search is read-only and no booking data is created. No write transaction is required. |
| Error Handling | Return MSG-BOOK-001 for invalid date range and MSG-MKT-001 for no matching hotels. |
| Privacy | Do not expose owner/staff private data, internal notes, audit records, or unpublished hotel configuration. |

## Assumptions and Open Issues

- ASSUMP-UC001-001: Exact API route, DTO field names, and repository names are conceptual because no codebase was inspected.
- ASSUMP-UC001-002: Marketplace search checks room type/date availability; physical room assignment is deferred to check-in/front desk flows.
- OQ-UC001-001: The SRS does not define every optional filter field beyond price, amenity, and availability filters.

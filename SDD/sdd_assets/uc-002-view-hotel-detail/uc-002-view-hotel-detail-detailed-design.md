# 3.2 UC-002 - View Hotel Detail

## 3.2.1 Design Purpose

This section describes the detailed design for **UC-002 View Hotel Detail**. Guest and Customer actors can open an approved hotel listing and view hotel profile, gallery, amenities, cancellation policy, room types, base prices, and availability information. The design is based on the SRS only; class names and methods are conceptual design assumptions and must be revalidated against the final codebase.

**Related SRS items:** FEAT-MKT, UC-002, SCR-006, ENT-005, ENT-006, ENT-007, ENT-008, ENT-009, ENT-010, ENT-011, ENT-012, BR-MKT-001, BR-AUTH-001, BR-ROOM-002, MSG-MKT-002, MSG-AUTH-002, TR-002, AT-UC002-02A, AT-UC002-05A.

The hotel detail flow must:

- Show only approved, active, and publicly available hotels under BR-MKT-001.
- Display SCR-006 gallery, hotel information, amenities, cancellation policy, room type list, and conditional select room action.
- Display room type capacity, base price, and selected-date availability when dates are provided.
- Enforce that Guest can view hotel detail, but Guest cannot create booking until login or registration under BR-AUTH-001.
- Allow Customer to proceed to booking from an available room type selection.
- Exclude blocked, inactive, occupied, dirty, cleaning, inspection-required, maintenance, or out-of-service rooms from availability under BR-ROOM-002.
- Ensure no booking data is created during hotel detail browsing or failed alternative paths.

## 3.2.2 Class Diagram

This part presents the class diagram for UC-002 View Hotel Detail.

![DGM-CLS-UC-002 - View Hotel Detail Class Diagram](./dgm-cls-uc-002-view-hotel-detail-class-diagram.png)

**Figure 3.2-1: Class Diagram of UC-002 View Hotel Detail**

## 3.2.3 Class Specifications

This part explains the key methods shown in the class diagram. The classes are conceptual design assumptions because no implementation codebase was inspected.

### HotelDetailController Class

**Description:** API entry point for hotel detail display and room selection handoff requests.

| No | Method | Description |
|---:|---|---|
| 1 | `getHotelDetail(request)` | Receives hotel detail input from SCR-006, validates it, calls `HotelDetailService`, and returns `HotelDetailResponse`. |
| 2 | `selectRoomType(request)` | Handles room selection and returns booking handoff for Customer or login/register guidance for Guest. |
| 3 | `validateDetailRequest(request)` | Checks hotel ID and optional stay date criteria before service execution. |

### HotelDetailRequest Class

**Description:** Request DTO carrying selected hotel ID, optional dates, guest count, and actor session context.

| No | Method | Description |
|---:|---|---|
| 1 | `hasHotelId()` | Confirms the selected hotel identifier is present. |
| 2 | `hasDateRange()` | Indicates whether selected stay dates were provided for availability display. |
| 3 | `hasValidDateRange()` | Confirms any provided check-out date is later than check-in date. |

### RoomSelectionRequest Class

**Description:** Request DTO carrying room type selection from SCR-006.

| No | Method | Description |
|---:|---|---|
| 1 | `isCustomerSession()` | Determines whether the actor is an authenticated Customer who can proceed to booking. |
| 2 | `hasSelectedRoomType()` | Confirms a room type was selected before booking handoff. |

### HotelDetailService Class

**Description:** Application service that coordinates public hotel lookup, content loading, room type display, availability summary, and booking handoff.

| No | Method | Description |
|---:|---|---|
| 1 | `getPublicHotelDetail(request)` | Executes the main UC-002 detail flow for approved public hotels. |
| 2 | `ensureHotelIsPublic(hotel)` | Enforces BR-MKT-001 before any hotel detail is displayed. |
| 3 | `buildDetailResponse(hotel, content, rooms, availability)` | Builds SCR-006 hotel profile, gallery, amenities, policy, room type list, and availability data. |
| 4 | `prepareBookingHandoff(selection)` | Prepares booking continuation for Customer without creating booking records in UC-002. |

### AvailabilityQueryService Class

**Description:** Service that calculates selected-date availability summaries for displayed room types.

| No | Method | Description |
|---:|---|---|
| 1 | `summarizeAvailability(roomTypeIds, dateRange)` | Builds room type availability summaries when selected dates are provided. |
| 2 | `countAvailableQuantity(roomTypeId, dateRange)` | Counts available quantity while excluding blocked, inactive, dirty, cleaning, inspection-required, maintenance, and out-of-service rooms. |
| 3 | `isRoomTypeAvailable(roomTypeId, dateRange)` | Confirms a room type can be selected for booking for the chosen dates. |

### HotelDetailResponse Class

**Description:** Response DTO returned to SCR-006 with public hotel content and availability display data.

| No | Method | Description |
|---:|---|---|
| 1 | `includeHotelProfile()` | Adds hotel name, address, description, and contact summary. |
| 2 | `includeGalleryAndAmenities()` | Adds active gallery images and amenities. |
| 3 | `includeRoomTypeSummaries()` | Adds room type capacity, base price, facilities, and availability summary. |
| 4 | `includeCancellationPolicy()` | Adds hotel-configurable cancellation policy summary. |

### HotelContentSummary Class

**Description:** Conceptual display aggregate that groups `HotelImage`, `Amenity`, `HotelAmenity`, and `CancellationPolicy` for readability.

| No | Method | Description |
|---:|---|---|
| 1 | `hasDisplayableImages()` | Confirms the hotel has active images for the gallery. |
| 2 | `hasActiveAmenities()` | Confirms active amenities are available for display. |
| 3 | `hasActivePolicy()` | Confirms cancellation policy summary is available. |

### HotelRepository Class

**Description:** Repository for retrieving marketplace-visible hotel property data.

| No | Method | Description |
|---:|---|---|
| 1 | `findPublicHotelById(hotelId)` | Finds a hotel by ID only when it is approved, active, and publicly available. |
| 2 | `findHotelContactSummary(hotelId)` | Loads public contact information for SCR-006. |

### HotelContentRepository Class

**Description:** Repository for gallery images, amenities, hotel-amenity associations, and cancellation policy summary.

| No | Method | Description |
|---:|---|---|
| 1 | `findActiveImages(hotelId)` | Loads ordered active hotel images. |
| 2 | `findActiveAmenities(hotelId)` | Loads active amenities associated with the hotel. |
| 3 | `findActiveCancellationPolicy(hotelId)` | Loads active hotel cancellation policy summary. |

### RoomTypeRepository Class

**Description:** Repository for retrieving room types shown or selected on SCR-006.

| No | Method | Description |
|---:|---|---|
| 1 | `findActiveRoomTypesByHotel(hotelId)` | Retrieves active private room types for the selected hotel. |
| 2 | `findRoomTypeByHotel(roomTypeId, hotelId)` | Confirms the selected room type belongs to the current hotel. |

### RoomAvailabilityRepository Class

**Description:** Repository for room type and physical room availability records.

| No | Method | Description |
|---:|---|---|
| 1 | `findAvailabilityByRoomTypes(roomTypeIds, dateRange)` | Loads availability records that overlap selected stay dates. |
| 2 | `countBlockedQuantity(roomTypeId, dateRange)` | Counts unavailable quantity caused by block records or excluded room statuses. |

### HotelProperty Class

**Description:** Domain entity representing a hotel listed on the marketplace.

| No | Method | Description |
|---:|---|---|
| 1 | `isApprovedActiveAndPublic()` | Returns whether the hotel can be displayed publicly. |

### RoomType Class

**Description:** Domain entity representing a private room type displayed for a hotel.

| No | Method | Description |
|---:|---|---|
| 1 | `isActive()` | Returns whether the room type can be displayed and selected. |
| 2 | `canFitGuestCount(guestCount)` | Checks whether room type capacity satisfies guest count. |

### RoomAvailability Class

**Description:** Domain entity representing availability or block status over a date range.

| No | Method | Description |
|---:|---|---|
| 1 | `overlaps(dateRange)` | Checks whether the record overlaps selected dates. |
| 2 | `isAvailable()` | Returns whether the record allows marketplace availability. |

## 3.2.4 Sequence Diagram

This part presents the sequence diagrams for UC-002 View Hotel Detail. The main-flow diagram shows only the successful scenario. Each alternative/error scenario has its own diagram.

![DGM-SEQ-UC-002 - View Hotel Detail Main Flow](./dgm-seq-uc-002-view-hotel-detail-main-flow.png)

**Figure 3.2-2: Sequence Diagram of UC-002 View Hotel Detail - Main Flow**

### AT-UC002-02A - Hotel No Longer Available

- **Branch from Main Step:** 2
- **Condition:** Hotel no longer available.
- **Expected Response:** This hotel is no longer available for public booking. The system returns the actor to UC-001 Search Hotels and no booking data is created.

![DGM-SEQ-UC-002 - Hotel No Longer Available](./dgm-seq-uc-002-view-hotel-detail-at-uc002-02a-hotel-no-longer-available.png)

**Figure 3.2-3: Sequence Diagram of UC-002 View Hotel Detail - AT-UC002-02A Hotel No Longer Available**

### AT-UC002-05A - Guest Selects Booking

- **Branch from Main Step:** 5
- **Condition:** Guest selects booking.
- **Expected Response:** Please log in or register before creating a booking. The system requires UC-003 Register Account or UC-004 Login before booking resumes.

![DGM-SEQ-UC-002 - Guest Selects Booking](./dgm-seq-uc-002-view-hotel-detail-at-uc002-05a-guest-selects-booking.png)

**Figure 3.2-4: Sequence Diagram of UC-002 View Hotel Detail - AT-UC002-05A Guest Selects Booking**

### Validation, Authorization, Transaction, and Error Handling Notes

| Area | Design |
|---|---|
| Validation | Validate hotel ID, optional check-in/check-out date range, guest count when present, and selected room type before booking handoff. |
| Authorization | Guest can view hotel detail. Guest cannot create booking until registration/login. Customer can proceed to booking from a valid room selection. |
| Transaction | UC-002 is read-only. Room selection prepares handoff only; no booking data is created in this use case. |
| Error Handling | Return MSG-MKT-002 when hotel is not public. Return MSG-AUTH-002 when Guest selects booking. |
| Privacy | Do not expose owner/staff private data, unpublished configuration, audit records, or non-public operational room details. |

## Assumptions and Open Issues

- ASSUMP-UC002-001: Exact API route, DTO field names, repository names, and method signatures are conceptual because no codebase was inspected.
- ASSUMP-UC002-002: `HotelContentSummary` groups `HotelImage`, `Amenity`, `HotelAmenity`, and `CancellationPolicy` in the class diagram to keep it readable while preserving traceability.
- ASSUMP-UC002-003: Room availability displayed on SCR-006 is a read-only room type availability summary; physical room assignment belongs to booking/front desk designs.
- OQ-UC002-001: The SRS does not specify whether SCR-006 displays room types without selected dates as “date required” or as base information without availability quantity.

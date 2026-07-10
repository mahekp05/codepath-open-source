# CodePath Open Source Contribution

## Contribution 1: Show Linked Barcode String in the User Interface

**Student:** Mahek Patel
**Contribution Number:** 1
**Project:** [InvenTree](https://github.com/inventree/InvenTree)
**Issue:** [inventree/InvenTree#11745](https://github.com/inventree/InvenTree/issues/11745)
**Status:** Phase IV Complete — Awaiting Review

## Why I Chose This Issue

I chose this issue because it addressed a user-facing feature with a clearly defined problem and expected outcome. In previous versions of InvenTree, users could view the barcode string linked to a stock item directly from its details page. Following a user interface update, this information was no longer visible, making manual barcode verification more difficult.

This contribution also aligned with my interests in full-stack development and user interface design. Through my previous work developing educational software, I have frequently connected backend data to frontend interfaces. This issue gave me the opportunity to apply those skills while learning how data models, serializers, frontend components, automated tests, and API versioning are managed within a large production codebase.

## Steps to Reproduce the Original Issue

1. Fork and clone the InvenTree repository.
2. Build and start the development environment using the provided Docker development setup.
3. Populate the database with sample inventory data.
4. Start the application and navigate to `http://localhost:8000`.
5. Log in using the development credentials.
6. Navigate to **Stock** and open a stock item.
7. Link a barcode to the stock item.
8. Open the **Stock Details** tab.
9. Observe that barcode-related actions are available, but the linked barcode string is not displayed in the details panel.

## Reproduction Evidence

The issue was successfully reproduced using a local InvenTree development environment and the provided test dataset.

The Stock Details panel displayed information such as the base part, status, supplier part, location, purchase order, unit price, and stock value. However, it did not display the barcode string linked to the stock item.

## Investigation

While investigating the barcode data flow, I found that the linked third-party barcode string is stored in the `barcode_data` field provided by `InvenTreeBarcodeMixin`, which is inherited by `StockItem`.

However, `StockItemSerializer` exposed `barcode_hash` without exposing `barcode_data`. As a result, the frontend did not receive the original human-readable barcode string and could not display it in the Stock Details panel.

## Implementation

To restore the linked barcode information, I:

* Added `barcode_data` as a read-only field on `StockItemSerializer`.
* Added a copyable **Linked Barcode** field to the Stock Details panel.
* Configured the field to appear only when a barcode is linked to the stock item.
* Preserved the existing barcode linking and unlinking behavior.
* Added a backend API test for the new serializer field.
* Added a Playwright test covering the barcode link, display, and unlink workflow.
* Increased the InvenTree API version to account for the newly exposed API field.

## Code Changes

The implementation includes changes to the following areas:

* `src/backend/InvenTree/stock/serializers.py`
* Backend stock API tests
* `src/frontend/src/pages/stock/StockDetail.tsx`
* Playwright stock item tests
* `src/backend/InvenTree/InvenTree/api_version.py`

**Development Branch:**
https://github.com/mahekp05/InvenTree/tree/fix-11745-linked-barcode-ui

## Testing Strategy

I validated the implementation by:

* Confirming that the updated stock item API response includes `barcode_data`.
* Confirming that `barcode_data` is read-only and cannot be modified through the stock item API.
* Linking a test barcode to a stock item and verifying that the expected value appears in the API response.
* Verifying that the frontend displays the **Linked Barcode** field when barcode data exists.
* Verifying that the field is hidden when no barcode is linked.
* Verifying the complete link, display, and unlink workflow through an automated Playwright test.
* Confirming that existing barcode functionality remains unchanged.
* Running the relevant formatting and pre-commit checks for the modified files.

# Pull Request

**PR Link:**
https://github.com/inventree/InvenTree/pull/12354

## PR Description

This pull request restores the linked third-party barcode string on the stock item detail page. The barcode string was visible in earlier versions of InvenTree but was no longer accessible from the details page after the user interface rewrite.

The contribution exposes `barcode_data` as a read-only field through `StockItemSerializer` and displays it as a copyable **Linked Barcode** field in the Stock Details panel. The field is conditionally shown only when the stock item has a linked barcode.

The pull request also includes backend and Playwright tests covering the new API behavior and the barcode link, display, and unlink workflow.

**Relevant Issue:**
Fixes [inventree/InvenTree#11745](https://github.com/inventree/InvenTree/issues/11745)

## Acceptance Criteria

* [x] The original issue can no longer be reproduced.
* [x] The linked barcode string is exposed through the stock item API.
* [x] The API field is read-only.
* [x] The Linked Barcode field is shown when a barcode exists.
* [x] The Linked Barcode field is hidden when no barcode exists.
* [x] Existing barcode link and unlink functionality remains unchanged.
* [x] Backend test added for the changed API behavior.
* [x] Playwright test added for the user workflow.
* [x] Relevant formatting and pre-commit checks completed.
* [x] No unrelated changes intentionally introduced.
* [x] API version updated for the newly exposed field.

## Maintainer Feedback

**July 10, 2026:** Submitted the pull request to the upstream InvenTree repository and requested a review from the relevant code owner.

No maintainer-requested changes have been received yet. The pull request is currently awaiting review. I will update this section with any reviewer feedback, changes made, and follow-up commits.

## Current Status

**Awaiting Review**

The pull request has been submitted to the upstream repository with implementation changes, automated tests, and an API version update. Phase IV is complete, and I will continue responding to feedback while the pull request is under review.

# Learnings and Reflections

The most important lesson from this contribution was that a small user interface change can require investigation across several layers of a production application.

Although the visible result was a single field in the Stock Details panel, implementing it required understanding:

* How barcode information is stored in the backend model.
* Which data is exposed through the serializer.
* How generated API types connect the backend and frontend.
* How the Stock Details panel conditionally renders fields.
* How backend and end-to-end tests are structured.
* When an API version must be increased after exposing a new field.

The most challenging part was navigating a large, unfamiliar codebase and tracing the barcode value from the data model to the user interface. Reading nearby implementations and existing tests helped me understand the project’s conventions more effectively than trying to design the solution independently.

I also learned that submitting a pull request is not the end of the contribution process. A professional open-source contribution includes documenting the motivation, adding appropriate tests, responding to automated checks, requesting review, and being prepared to iterate based on maintainer feedback.

For my next contribution, I would examine the relevant serializer tests, frontend test utilities, and project contribution requirements earlier in the process. This would help me plan the implementation and testing strategy together rather than treating testing as a separate final step.

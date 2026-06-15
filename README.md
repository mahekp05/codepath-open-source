# codepath-open-source

# Contribution 1: Show Linked Barcode String in User Interface

**Contribution Number:** 1
**Student:** Mahek Patel
**Issue:** https://github.com/inventree/InvenTree/issues/11745
**Status:** Phase I – In Progress

---

## Why I Chose This Issue

I chose this issue because it appears to be a user-facing feature request with a clearly defined problem and expected outcome. Users previously had access to barcode strings directly from the stock item details page and relied on this information for manual verification processes. Restoring this functionality provides a meaningful improvement to the user experience while allowing me to contribute to a production open-source application.

This issue also aligns with my interests in full-stack development and user interface design. Through my work developing educational software at the University of South Florida, I have frequently worked on connecting backend data to frontend interfaces. By contributing to this issue, I hope to gain experience navigating a larger codebase, understanding existing data models, and implementing UI enhancements that support real-world workflows.

## Steps to Reproduce

1. Fork and clone the InvenTree repository.
2. Build and start the development environment using the provided Docker development setup.
3. Run the test data setup command to populate the database with sample inventory data.
4. Start the application and navigate to `http://localhost:8000`.
5. Log in using the test credentials.
6. Navigate to **Stock** and open any stock item.
7. Open the **Stock Details** tab.
8. Observe that barcode-related actions are available through the barcode dropdown menu, but the linked barcode string itself is not displayed anywhere in the Stock Details panel.

## Reproduction Evidence
Branch: https://github.com/mahekp05/InvenTree/tree/fix-11745-linked-barcode-ui
Issue reproduced successfully on a local development instance using the provided test dataset. The Stock Details page displays fields such as Base Part, Status, Supplier Part, Location, Purchase Order, Unit Price, and Stock Value, but does not display the linked barcode string.

## Implementation Plan

* Investigate how barcode information is currently retrieved and displayed within the `BarcodeActionDropdown` component.
* Identify the field or API property that contains the human-readable barcode string.
* Update `src/frontend/src/pages/stock/StockDetail.tsx` to include a Barcode field in the Stock Details panel.
* Ensure the barcode field is only shown when a barcode is linked to the stock item.
* Verify that the barcode string is visible on the Stock Details page and that existing barcode functionality remains unchanged.

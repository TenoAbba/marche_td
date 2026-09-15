# marche_td UI Design Handoff

This document is the Figma source of truth for recreating the existing Flutter app. Use the existing Flutter widgets and bundled assets as the visual authority. Do not redesign or invent alternate layouts.

## Product Surface

- Product name: `marche_td`
- Platform baseline: mobile, portrait
- Primary Figma frame: `390 x 844 px`
- Secondary validation frame: `393 x 852 px`
- Safe area: reserve the device status bar and bottom gesture area; app content uses Flutter `SafeArea`.
- Layout direction: LTR by default; the app supports RTL locales.
- Primary font: Manrope, bundled in `assets/fonts/`.

## Design Tokens

### Colors

| Token | Value | Use |
|---|---:|---|
| `background.light` | `#F6F5FA` | Page background |
| `surface.light` | `#FFFFFF` | Cards, fields, sheets |
| `primary` | `#00B2CA` | Main actions, active icons, prices, focus |
| `accent` | `#FA6E53` | Secondary action and emphasis |
| `text.light` | `#000000` | Primary light-theme text |
| `text.light.muted` | `rgba(0,0,0,0.50)` | Hints and secondary text |
| `border.light` | `rgba(238,238,238,0.60)` | Field/card borders |
| `background.dark` | `#121212` | Dark-theme page background |
| `surface.dark` | approximately `#1C1C1C` | Dark-theme cards and fields |
| `text.dark` | `#FDFDFD` | Primary dark-theme text |
| `text.dark.muted` | `rgba(253,253,253,0.30)` | Dark-theme secondary text |
| `error` | `#A60404` | Validation and error states |
| `warning` | `#C2AF6F` | Warning state |
| `success` | `#00B2CA` | Success state |

### Type Scale

All text uses Manrope with letter spacing at the default value.

- `10 px`: labels, category captions, compact metadata
- `12 px`: secondary text, helper text, small buttons
- `14 px`: standard body and input text
- `16 px`: emphasized body and large controls
- `18 px`: section titles and larger controls
- `24 px`: screen headings
- `28 px`: extra-large display text

Use weight 400 for body, 500 for medium labels, 600 for emphasis, 700 for prices/headings, and 800 only for high emphasis.

### Geometry

- Page horizontal padding: `18 px`
- Search field height: `56 px`
- Search field radius: `10 px`
- Category tile: `70 x 70 px`, tile radius `18 px`
- Listing card: `124 px` high, radius `15 px`
- Listing image: `100 px` wide, card-height image, radius `15 px`
- Favorite button: `32 x 32 px`, circular
- Standard vertical list spacing: `4.5 px` between listing cards
- Primary controls: use teal fill with white text where the Flutter screen uses an action button
- Borders: 1 px, low-contrast neutral border
- Elevation: whisper-soft shadow on light-theme floating controls; no heavy shadows

## Core Component Inventory

Create these as Figma components with variants for light/dark, enabled/disabled, selected/unselected, loading/error, and guest/authenticated states.

- `AppBar / Location`: transparent app bar with location icon, current location text, and page gutter.
- `SearchField`: 56 px white surface, 1 px border, 10 px radius, teal search icon, translated placeholder.
- `CategoryTile`: 70 px visual tile with 18 px radius and a centered image/icon; caption below or within the tile column.
- `ListingCard / Horizontal`: 124 px white surface, 15 px radius, 100 px image at left, teal price, title, metadata, circular favorite action.
- `FavoriteButton`: 32 px circular white surface with teal outline/fill heart icon.
- `BottomNavigation`: four destinations plus a centered raised add action; active icon uses teal.
- `PrimaryButton`: full-width or content-width teal action with white label.
- `TextField`: white surface, neutral border, 10 px radius, 56 px minimum height.
- `BlurredDialog`: centered modal over a blurred/dimmed page with illustration, title, body, and action controls.
- `Loading / Empty / Error`: use existing shimmer and illustration assets; preserve the same page padding.

## Authenticated Shell

Source: `lib/Ui/screens/main_activity.dart`.

Frame structure, top to bottom:

1. Device safe area.
2. `PageView` content area, full width.
3. Fixed bottom navigation region.
4. Centered raised add button overlapping the navigation region.
5. Bottom safe area.

Navigation order is fixed:

1. Home
2. Chat
3. My Ads
4. Profile

The center add action opens the sell flow and must remain visually dominant over the navigation bar. Do not convert this into a normal fifth tab.

## Home Screen

Source: `lib/Ui/screens/Home/home_screen.dart`.

Frame structure, top to bottom:

1. Safe area.
2. Transparent app bar with `LocationWidget`, aligned to the left with an 18 px gutter.
3. Main page background `#F6F5FA` in light mode.
4. Search field: 18 px horizontal inset, 15 px vertical spacing, 56 px high.
5. Promotional slider/banner below the search field.
6. Horizontal category strip: repeated 70 px category tiles with captions.
7. Repeated content sections from the API. Each section has a section heading and its item layout.
8. Optional advertising banner between sections.
9. Bottom navigation shell.

The Home screen is scrollable. Preserve the vertical sequence exactly: location, search, slider, categories, sections, optional ad banner, more sections.

## Listing Card Layout

Source: `lib/Ui/screens/Home/Widgets/item_horizontal_card.dart`.

Inside each 124 px card:

- Left: clipped listing image, 100 px wide.
- Top-left image overlay: promoted icon when applicable.
- Right content padding: 12 px start/end and 5 px bottom.
- First row: teal price at 16 px / bold, favorite button aligned right.
- Below price: listing title at standard body size.
- Remaining rows: location, category, date, or seller metadata depending on state.
- Optional status badge appears below the image for My Ads states.

## Search and Filter Flow

Frames:

- Search: top app bar, search input, recent/search results list.
- Filter: category, subcategory, location, price, and posted-since controls in a vertical scroll.
- Category picker: selectable rows with trailing navigation affordance.
- Subcategory picker: same row structure, selected state in teal.
- Posted since: single-choice list with selected indicator.

Use `assets/svg/search.svg`, `filter.svg`, `filter_by.svg`, `sort_by.svg`, and `since_icon.svg`.

## Item Details

Source: `lib/Ui/screens/ad_details_screen.dart`.

Frame structure:

1. App bar with back navigation and item actions.
2. Large media/gallery region.
3. Price and title block.
4. Location and metadata block.
5. Description and custom fields.
6. Seller summary and contact/chat actions.
7. Favorite/share actions.
8. Fixed or end-of-content primary contact action according to the Flutter implementation.

Use `gallery_view.dart` and the existing image viewer behavior. Keep the media region visually dominant; do not replace it with a generic card grid.

## Authentication Screens

Primary frames:

- Login: logo/brand area, email or phone field, password field, primary login action, social login actions, forgot password, signup link.
- Signup main: method selection and social/email/phone choices.
- Email signup: stacked form fields and primary action.
- Mobile signup: country selector, phone field, verification action.
- Forgot password: email/phone field and recovery action.

Use the shared `CustomTextFormField` geometry. Keep form content within the 18 px page gutter and preserve the large vertical whitespace around the form.

Assets: `assets/svg/Logo/`, `assets/svg/google_icon.svg`, `assets/svg/apple_icon.svg`.

## Sell Flow

This is a sequential full-screen flow, not a modal wizard:

`Select category` -> `Select nested category` -> `Add item details` -> `More details` -> `Confirm location` -> `Success`.

Each frame uses:

- Back arrow app bar.
- Screen title.
- Scrollable form/content area.
- Teal primary action near the bottom of the content.
- Validation and loading variants.

Preserve the route names and order from `lib/app/routes.dart`.

## Chat

- Chat list: app bar, conversation rows with avatar, name, last message, time, and unread state.
- Chat detail: app bar with seller identity, message list, attachment actions, text composer, send action.
- Message bubbles use the light/dark sender colors from `theme.dart`.
- Use `assets/chat_background/light.svg` for the light chat background.
- Attachment icons: `attachment.svg`, `camara_image_attach.svg`, `gallery_image_attach.svg`, `document_attach.svg`.

## My Ads and Profile

My Ads uses the same `ListingCard / Horizontal` component with status variants for pending, sold, deactivated, and active listings.

Profile structure:

1. Profile header with avatar and identity.
2. Edit profile action.
3. Account/settings rows.
4. Notifications, language, subscriptions, transaction history, FAQs, contact, and logout.

Use the existing icons in `assets/svg/`: `edit_profile.svg`, `notification.svg`, `language.svg`, `subscription.svg`, `transaction.svg`, `faqs.svg`, `contact_us.svg`, and `logout.svg`.

## Required Figma Pages

Create these Figma pages in this order:

1. `00 Cover + Tokens`
2. `01 Components`
3. `02 Auth Flow`
4. `03 Home + Browse`
5. `04 Item Details`
6. `05 Sell Flow`
7. `06 Chat`
8. `07 My Ads + Profile`
9. `08 States`

On every page, use the 390 x 844 mobile frame and name frames after the Flutter route/screen, for example `HomeScreen`, `LoginScreen`, `AdDetailsScreen`, and `SelectCategoryScreen`.

## Source Mapping

- Routing: `lib/app/routes.dart`
- App shell: `lib/Ui/screens/main_activity.dart`
- Home: `lib/Ui/screens/Home/home_screen.dart`
- Search field: `lib/Ui/screens/Home/Widgets/home_search.dart`
- Category tile: `lib/Ui/screens/Home/Widgets/category_home_card.dart`
- Listing card: `lib/Ui/screens/Home/Widgets/item_horizontal_card.dart`
- Theme: `lib/Ui/Theme/theme.dart`, `lib/app/app_theme.dart`
- Font files: `assets/fonts/Manrope-*.ttf`
- SVG assets: `assets/svg/`
- Onboarding illustrations: `assets/svg/Illustrators/onbo_a.svg`, `onbo_b.svg`, `onbo_c.svg`
- Bottom navigation assets: `assets/svg/bottomnav/`

## Fidelity Rules

- Do not substitute Inter, Roboto, or system fonts for Manrope.
- Do not change the 18 px page gutter, 56 px search height, 70 px category tile, or 124 px listing card dimensions.
- Do not replace the center raised add action with a standard tab.
- Do not flatten the Home screen into a generic marketplace grid; preserve its slider, horizontal categories, and API-driven section sequence.
- Use the bundled SVG and font assets rather than redrawing approximate icons.
- Build light and dark variants from the same component set; teal and coral remain the brand accents in both modes.

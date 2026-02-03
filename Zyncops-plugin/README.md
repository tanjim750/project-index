# ZyncOps

ZyncOps is a WooCommerce extension that centralizes courier automation, abandoned cart recovery, and risk signals for Bangladeshi e-commerce stores. It adds a dedicated Zyncops admin menu with dashboards and configuration screens for courier integrations, Facebook tracking, order distribution, and order restrictions.

The plugin requires a valid license key to activate most features. Once activated, it adds background checks for license validation and optional courier status updates, and stores customer/order footprint data to support cart recovery and restriction rules.

**Features**
1. Courier integrations for Redx, Steadfast, and Pathao with order creation, tracking, and status updates.
2. Auto order status updates based on courier tracking.
3. Abandoned cart tracking with customer footprint and recovery tooling.
4. Order restriction rules (IP or user-based limits, success ratio, and temporary restriction status).
5. Order distribution helpers and admin tools for managing fulfillment flow.
6. Facebook Pixel and server-side event dispatch for key WooCommerce actions.
7. Custom order status creation and custom event mapping.
8. Sales report dashboard with line and pie chart views.

**Requirements**
1. WordPress with WooCommerce installed and active.
2. A valid ZyncOps license key.

**Installation**
1. Copy the `ZyncOps` folder into `wp-content/plugins/`.
2. Activate the plugin from the WordPress admin Plugins screen.

**Setup And Configuration**
1. Go to `Zyncops` in the WordPress admin menu.
2. Open `General`, enable the plugin, and enter your ZyncOps license key (required to activate features).
3. Configure courier integration in `Courier > Configuration` to select a provider and enable auto status updates, then open the provider pages (`Redx`, `Steadfast`, `Pathao`) to set API credentials and defaults.
4. Configure Facebook tracking in `Facebook Tracking` (Pixel ID, access token, test event code), and use `Add Event` to map WooCommerce actions to custom events if needed.
5. Configure order flow in `Order Distribution` (routing/assignment rules) and `Order Restriction` (limits and enforcement method).
6. Configure cart recovery in `Cart Abandonment` to review and manage abandoned sessions.
7. Review analytics in `Sales Report` for order trends and status breakdowns.

**Data Storage**
1. Creates two custom tables on activation: `wp_zyncops_customer` for visitor identity, limits, and status, and `wp_zyncops_order` for abandoned and completed order tracking.
2. Uses `wp_options` for plugin settings, license state, and feature toggles.

**Operational Notes**
1. The plugin periodically validates the license with the ZyncOps service.
2. Courier tracking updates run in the background based on admin or front-end page activity.
3. Facebook events are sent through both Pixel scripts and server-side requests when configured.

**Project Structure**
1. `zyncops.php`: plugin bootstrap and core hooks.
2. `includes/settings`: admin pages and settings handlers.
3. `includes/wc`: WooCommerce integration logic.
4. `includes/models`: custom data tables and access helpers.
5. `includes/tracking`: Facebook Pixel and event utilities.
6. `assets`: admin and front-end scripts and styles.


# Module 2: Setting up Google Analytics 4 (GA4)

## Introduction to Google Analytics 4

Google Analytics 4 (GA4) is the latest version of Google's web analytics service. It represents a significant shift from the previous version, Universal Analytics (UA). Key differences include:

*   **Event-based Model:** Unlike UA's session-based model, GA4 uses an event-based model. Every user interaction is captured as an event (e.g., `page_view`, `scroll`, `click`, `purchase`). This provides a more flexible and granular way to track user behavior across platforms.
*   **Cross-platform Tracking:** GA4 is designed for a multi-platform world, allowing you to track users seamlessly across websites and mobile apps within a single property.
*   **Privacy-focused:** GA4 is built with privacy at its core, offering features like cookieless measurement and behavioral modeling for consented users.
*   **AI-powered Insights:** It leverages Google's machine learning capabilities to provide predictive metrics and automated insights.
*   **BigQuery Integration:** GA4 offers a free, native integration with BigQuery, allowing you to access raw event data for deeper analysis (which we'll cover in this module).

Universal Analytics properties stopped processing new data on July 1, 2023. Therefore, migrating to and understanding GA4 is crucial for anyone relying on web analytics.

## Creating a New GA4 Property

If you don't already have a GA4 property, here's how to create one:

1.  **Sign in to Google Analytics:** Go to [https://analytics.google.com/](https://analytics.google.com/) and sign in with your Google account.
2.  **Navigate to Admin:** Click on the "Admin" gear icon in the bottom-left corner of the screen.
3.  **Create Account (if needed):** If you don't have an existing Analytics account, click "+ Create Account." Provide an account name (e.g., "My Business"). Configure data sharing settings as desired and click "Next."
4.  **Create Property:**
    *   In the "Property" column, click "+ Create Property."
    *   **Property name:** Enter a name for your property (e.g., "My Website GA4").
    *   **Reporting time zone:** Select your reporting time zone.
    *   **Currency:** Select the currency your business operates in.
    *   **Advanced options (Universal Analytics):** Note that the option to create a Universal Analytics property is no longer available for new properties.
5.  **Click "Next."**
6.  **Business information:** Provide information about your business category, size, and how you intend to use Google Analytics. This helps tailor your experience.
7.  **Click "Create."** Accept the Google Analytics Terms of Service Agreement.

You have now successfully created a GA4 property. The next step is to set up a data stream.

## Setting Up Data Streams

A data stream is a source of data that flows into your GA4 property. You'll create a separate data stream for each website or app you want to track.

1.  **Choose a platform:** After creating your property, you should be prompted to "Choose a platform." You can select "Web," "Android app," or "iOS app."
    *   If you're not automatically prompted, go to **Admin > Property column > Data Streams > Add stream**.
2.  **Set up a Web stream (Example):**
    *   **Website URL:** Enter the URL of your website (e.g., `www.example.com`).
    *   **Stream name:** Give your stream a descriptive name (e.g., "My Website Stream").
    *   **Enhanced measurement:** This is enabled by default and automatically tracks common events like page views, scrolls, outbound clicks, site search, video engagement, and file downloads. You can configure this by clicking the gear icon.
3.  **Click "Create stream."**
4.  **Installation instructions:** After creating the stream, you'll see "Installation instructions." This provides your "Measurement ID" (looks like `G-XXXXXXXXXX`) and options for adding the GA4 tag to your website:
    *   **Install with a website builder or CMS:** If your website uses a platform like WordPress, Shopify, Wix, etc., there are often specific integrations or plugins to easily add your GA4 Measurement ID.
    *   **Install manually:** You'll be given a Google tag (gtag.js) snippet. This JavaScript code needs to be added to the `<head>` section of every page on your website.
    ```html
    <!-- Google tag (gtag.js) -->
    <script async src="https://www.googletagmanager.com/gtag/js?id=G-MEASUREMENT_ID"></script>
    <script>
      window.dataLayer = window.dataLayer || [];
      function gtag(){dataLayer.push(arguments);}
      gtag('js', new Date());

      gtag('config', 'G-MEASUREMENT_ID');
    </script>
    ```
    Replace `G-MEASUREMENT_ID` with your actual Measurement ID.

Once the tag is correctly installed, your GA4 property will start receiving data from your website. It can take 24-48 hours for data to begin appearing in your reports.

## Linking GA4 to BigQuery

One of the most powerful features of GA4 is its native integration with BigQuery. This allows you to export raw event data from your GA4 property to a BigQuery dataset, enabling more complex analysis and data warehousing.

1.  **Navigate to Admin:** In your GA4 property, click on "Admin" (gear icon).
2.  **Select your GA4 Property:** Ensure the correct GA4 property is selected in the "Property" column.
3.  **Find BigQuery Links:** In the "Property" column, under "Product Links," click on "BigQuery Links."
4.  **Click "Link":** Click the blue "Link" button. If you have multiple BigQuery projects, ensure you have the correct one selected.
5.  **Choose a BigQuery project:** Click "Choose a BigQuery project." A list of BigQuery projects you have access to will appear. Select the project where you want to export your GA4 data.
    *   **Note:** You need at least `Editor` role on the GCP project and `Owner` role on the BigQuery project (or specific BigQuery permissions like `bigquery.dataEditor` and `bigquery.user`) to link GA4 to BigQuery. The service account `firebase-measurement@system.gserviceaccount.com` also needs to be granted `BigQuery Data Editor` role on the chosen dataset.
6.  **Click "Confirm."**
7.  **Configure data streams and events:**
    *   **Data streams:** Select the data stream(s) you want to export data from (e.g., your "My Website Stream").
    *   **Frequency:**
        *   **Daily:** Exports data once a day. A new table named `events_YYYYMMDD` is created for each day.
        *   **Streaming:** Exports data within minutes of events being collected. Creates a table named `events_intraday_YYYYMMDD` that is updated throughout the day, and then a stable `events_YYYYMMDD` table is created at the end of the day. Using the streaming export has an additional cost in BigQuery.
    *   **Include advertising identifiers:** (Optional) If you need to include advertising identifiers (like Google Click ID or IDFA) in your export, you can check this box. Be mindful of privacy implications.
8.  **Click "Next."**
9.  **Review and submit:** Review your configuration settings.
10. **Click "Submit."**

Your GA4 property is now linked to BigQuery! Data will start populating in your selected BigQuery dataset according to the frequency you chose. Typically, a new dataset named `analytics_<property_id>` will be automatically created in your BigQuery project, and within that, tables like `events_YYYYMMDD` will appear.

## Benefits of GA4-BigQuery Integration

*   **Access to Raw Event Data:** Get unsampled, granular data for every event, providing a complete picture of user interactions.
*   **Advanced Analysis Capabilities:** Perform complex SQL queries, join GA4 data with other data sources (e.g., CRM, sales data), and build custom attribution models.
*   **Data Ownership and Control:** Your data is stored in your own BigQuery project, giving you full control over it.
*   **Overcome GA4 Interface Limitations:** The GA4 interface has certain reporting limitations and sampling at high data volumes. BigQuery allows you to bypass these.
*   **Data Warehousing:** Use BigQuery as a central data warehouse for all your marketing and business data.
*   **Machine Learning:** Leverage BigQuery ML to build machine learning models directly on your GA4 data.

This module has guided you through setting up GA4 and linking it to BigQuery, unlocking powerful analytical capabilities. The next module will focus on understanding the GA4 event schema in BigQuery.

# Module 7: Working with Dialogflow CX - Agents and Flows

This module delves into the practical aspects of creating and structuring conversational experiences in Dialogflow CX, focusing on Agents, Flows, and Pages, and how they come together in the visual flow builder.

## Agents: The Top-Level Container

An agent is the highest-level container in Dialogflow CX. It encompasses all the flows, pages, intents, entities, and settings for your conversational AI application.

### Creating a New Dialogflow CX Agent

1.  **Go to the Dialogflow CX Console:** Open your web browser and navigate to [https://dialogflow.cloud.google.com/cx/](https://dialogflow.cloud.google.com/cx/).
2.  **Select your Google Cloud Project:** Ensure the correct GCP project is selected in the console. You might be prompted to select one if it's your first time.
3.  **Create Agent:** Click the "+ Create agent" button.
    `[Screenshot: Dialogflow CX console - Agent selection page with "+ Create agent" button highlighted.]`
4.  **Agent Configuration:**
    *   **Agent name:** Enter a descriptive name for your agent (e.g., "Customer Service Bot," "Appointment Scheduler").
    *   **Location:** Choose the GCP region where your agent will be hosted. This affects latency and data residency. Select a location closest to your users or backend services.
        `[Screenshot: Dialogflow CX "Create agent" dialog - Name and Location fields.]`
    *   **Default language:** Select the primary language your agent will use. You can add more languages later.
    *   **Time zone:** Select the default time zone for your agent. This is important for date/time related operations.
    *   **Lock agent (Optional):** If multiple people are working on the agent, you might want to lock it to prevent simultaneous edits if needed.
    *   **Advanced settings (Optional):** You can configure logging settings and other advanced options here or later.
5.  **Click "Create."**

It will take a few moments for your agent to be created. Once done, you'll be taken to the agent's main interface, usually starting in the "Build" tab.

### Agent Settings Overview

Once your agent is created, you can access its settings:

1.  In the left navigation panel of your agent, click on "Agent Settings."
    `[Screenshot: Dialogflow CX agent interface - "Agent Settings" in the left navigation panel highlighted.]`
2.  Here you can modify:
    *   **General:** Agent name, description, default language, time zone, avatar.
    *   **Logging:** Enable/disable interaction logging and Cloud Logging.
    *   **ML Settings:** Configure Natural Language Understanding (NLU) thresholds and other machine learning parameters.
    *   **Speech and IVR:** Settings related to speech synthesis and telephony integrations.
    *   **Advanced:** Options like agent validation and custom payload templates.

### Understanding the Default Start Flow

When you create a new agent, Dialogflow CX automatically creates a "Default Start Flow."
*   This flow is the entry point for all new conversations with your agent.
*   You cannot delete the Default Start Flow, but you can (and should) customize it.
*   It contains a "Start Page" which is the very first page a user lands on.

## Flows: Defining Conversational Topics

Flows are the building blocks for organizing conversation topics within your agent. Think of them as distinct sub-conversations or modules. For example, an e-commerce agent might have flows for "New Order," "Track Order," "Returns," and "Customer Support."

### Creating a New Flow

1.  **Navigate to the "Build" Tab:** Ensure you are in the "Build" section of your agent.
2.  **Access Flows List:** In the "Flows" panel (usually on the left), you'll see the "Default Start Flow."
3.  **Click "+ Create flow" or the "+" icon next to "Flows":**
    `[Screenshot: Dialogflow CX "Build" tab - "Flows" panel with "+" icon highlighted.]`
4.  **Enter Flow Name:** Give your flow a descriptive name (e.g., "Order Management," "Product Questions").
    `[Screenshot: "Create new flow" dialog box.]`
5.  **Click "Save."**

Your new flow will appear in the Flows list. You can now select it to start building its conversational path.

### The Concept of the Start Page within a Flow

*   Each flow you create will also have a default "Start Page" (often just named "Start").
*   This is the initial page a user lands on when the conversation transitions into this particular flow.
*   You can customize this Start Page or designate another page within the flow to be its entry point if needed (though typically the default Start Page is used as the entry).

### Renaming and Deleting Flows

*   **Rename:** Hover over a flow in the "Flows" panel, click the three vertical dots (options menu), and select "Rename flow."
*   **Delete:** Hover over a flow, click the options menu, and select "Delete flow." Be cautious, as this will remove all pages and associated configurations within that flow. You cannot delete the "Default Start Flow."

    `[Screenshot: Flow options menu showing "Rename flow" and "Delete flow".]`

### Flow-Level Settings

Each flow can have its own settings, such as event handlers that apply to all pages within that flow. You can access these by selecting the flow and then looking for options to add event handlers or manage flow-specific NLU settings if applicable.

## Pages: Representing Conversational States

Pages represent individual states or steps within a flow. Each page is responsible for gathering information from the user, providing information, or making decisions about where the conversation should go next.

### Creating New Pages within a Flow

1.  **Select a Flow:** In the "Flows" panel, click on the flow where you want to add a page.
2.  **Access the Visual Builder:** The central part of the screen will show the visual flow builder for the selected flow.
3.  **Add a Page:** Click the "+" (Add page) button in the flow builder's toolbar or sometimes available by right-clicking on the canvas.
    `[Screenshot: Dialogflow CX visual flow builder - "+" (Add page) button highlighted.]`
4.  **Name the Page:** A new page node will appear. Click on its default name (e.g., "New Page") to rename it to something descriptive (e.g., "Collect Address," "Confirm Order," "FAQ Display").

### Key Components of a Page

When you select a page in the visual builder, its configuration panel opens (usually on the right).

`[Screenshot: Page configuration panel showing Entry fulfillment, Parameters, and Routes sections.]`

*   **Entry fulfillment (On enter):**
    *   Actions to perform immediately when the conversation transitions to this page.
    *   Common uses:
        *   Sending a message to the user (e.g., "What is your order number?").
        *   Calling a webhook to fetch data.
        *   Setting session parameters.
    *   You can define static text responses, call a webhook, or set parameter presets.

*   **Parameters (Form Filling):**
    *   Used to collect specific pieces of information (entities) from the user while they are on this page.
    *   For each parameter you want to collect:
        *   Define a display name.
        *   Select the entity type (e.g., `@sys.number`, `@sys.date`, your custom entities).
        *   Write a prompt that the agent will use to ask the user for this information (e.g., "What's your phone number?").
    *   The page will keep prompting for required parameters until they are filled or a condition/intent routes the conversation elsewhere.

*   **Routes (State Handlers):**
    *   Define the logic for transitioning from the current page to another page or flow. Routes are evaluated in order.
    *   **Intent Routes:** Triggered when a specific user intent is matched.
        *   Condition: `intent = <your_intent_name>`
        *   Action: Transition to a target page/flow, or call a webhook.
    *   **Condition Routes:** Triggered if a condition based on session parameters is met.
        *   Condition: `session.params.<param_name> = "value"`
        *   Action: Transition, webhook.
    *   **Event Routes:** Triggered by system events (e.g., `sys.no-match-default`, `sys.no-input-default`) or custom events.
        *   Event: `sys.no-match-default`
        *   Action: Send a message like "I didn't understand that. Could you rephrase?"

### Setting a Page as the Start Page of a Flow

*   Typically, the first page created in a flow (often named "Start") is automatically the flow's start page.
*   If you need to change it:
    1.  Select the flow in the "Flows" panel.
    2.  In the visual builder for that flow, find the page you want to set as the start page.
    3.  Right-click on the page node or find its options menu (three dots).
    4.  Select "Set as flow start page."
        `[Screenshot: Right-click menu on a page node in the visual builder, with "Set as flow start page" highlighted.]`

## Visual Flow Builder Navigation

The visual flow builder is the heart of Dialogflow CX design:

`[Screenshot: Overview of the Dialogflow CX visual flow builder interface, highlighting the canvas, page nodes, and connection lines.]`

*   **Canvas:** The main area where you see your pages as nodes and their connections.
*   **Adding Pages:** Use the "+" button.
*   **Connecting Pages:**
    *   When you define a route (intent or condition) on a page that transitions to another page, a line will automatically be drawn.
    *   You can often also click and drag from a route's output handle on one page to the input handle of another page to create or modify transitions.
*   **Zoom and Pan:** Use mouse scroll to zoom in/out and click-drag on the canvas to pan.
*   **Page Configuration:** Clicking a page node opens its detailed configuration panel.
*   **Flow-level Elements:** You can also add flow-level event handlers or route groups that appear on the canvas.

The builder helps you visualize the entire conversation structure, making it easier to understand and debug complex interactions.

## Practical Example: Simple Agent Structure

Let's outline a very basic agent structure for a simple inquiry:

1.  **Agent:** "Simple Inquiry Agent"
2.  **Default Start Flow:**
    *   **Start Page:**
        *   Entry Fulfillment: "Welcome! How can I help you today?"
        *   Routes:
            *   Intent: `greet` -> Transition to "Main Menu Page" in "Main Menu Flow."
            *   Intent: `ask_order_status` -> Transition to "Get Order ID Page" in "Order Status Flow."
3.  **Flow: "Main Menu Flow"**
    *   **Start Page (e.g., "Main Menu Page"):**
        *   Entry Fulfillment: "You can ask about order status or FAQs."
        *   Routes:
            *   Intent: `ask_order_status` -> Transition to "Get Order ID Page" in "Order Status Flow."
            *   Intent: `ask_faq` -> Transition to "Show FAQ Page" in "FAQ Flow."
4.  **Flow: "Order Status Flow"**
    *   **Start Page (e.g., "Get Order ID Page"):**
        *   Entry Fulfillment: "Sure, I can help with that. What's your order ID?"
        *   Parameters: `order_id` (of type `@sys.number`)
        *   Routes:
            *   Condition: `session.params.order_id` is filled -> Transition to "Lookup Order Page."
    *   **Page: "Lookup Order Page":**
        *   Entry Fulfillment: Call a webhook with `order_id` to get status.
        *   Routes (based on webhook response):
            *   Condition: `session.params.order_status = "shipped"` -> Transition to "Shipped Info Page."
            *   Condition: `session.params.order_status = "pending"` -> Transition to "Pending Info Page."
    *   **Page: "Shipped Info Page":** Entry Fulfillment: "Your order #$session.params.order_id has shipped!"
    *   **Page: "Pending Info Page":** Entry Fulfillment: "Your order #$session.params.order_id is still pending."

This structure demonstrates how flows can separate concerns and how pages manage individual steps within those concerns. The visual builder would clearly show these flows and the connections between their pages.

This module provides the foundational knowledge for structuring your Dialogflow CX agent. The next steps involve defining intents, entities, and more complex fulfillment logic.

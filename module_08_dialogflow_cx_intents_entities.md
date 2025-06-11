# Module 8: Working with Dialogflow CX - Intents and Entities

Intents and Entities are the core components of Natural Language Understanding (NLU) in Dialogflow CX. They allow your agent to understand what users say and extract meaningful information from their input.

## Intents: Understanding User Goals

An **intent** represents the user's intention or what they want to achieve when they say something. Dialogflow CX maps user utterances (what the user types or says) to the most appropriate intent in your agent.

### Creating a New Intent

1.  **Navigate to the "Build" Tab:** Ensure you are in the "Build" section of your agent.
2.  **Access Intents List:** In the left-hand navigation panel, under your agent's resources, click on "Intents."
    `[Screenshot: Dialogflow CX "Build" tab - "Intents" in the left navigation panel highlighted.]`
3.  **Click "+ Create intent" or the "+" icon next to "Intents":**
4.  **Enter Intent Name:** Give your intent a clear, descriptive name (e.g., `order.check_status`, `faq.billing`, `navigation.go_back`). Use a consistent naming convention (e.g., `noun.verb_phrase` or `verb.noun_phrase`).
    `[Screenshot: "Create new intent" dialog box with the "Intent name" field.]`
5.  **Click "Save."** This will take you to the intent configuration page.

### Training Phrases

Training phrases are examples of what a user might say to express a particular intent. Providing a diverse and sufficient set of training phrases is crucial for the accuracy of your agent's NLU.

*   **Importance:**
    *   **Variety:** Include different sentence structures, phrasings, and synonyms.
    *   **Quantity:** More phrases generally lead to better accuracy, but quality and variety are more important than sheer numbers. Aim for at least 10-20 varied phrases for common intents.
*   **Adding Training Phrases:**
    1.  On the intent configuration page, find the "Training phrases" section.
    2.  Click on "Add training phrases" or type directly into the "Enter user utterance" field.
    3.  Press Enter after each phrase.
        `[Screenshot: Intent configuration page - "Training phrases" section with input field highlighted.]`
    *   **Examples for an `order.check_status` intent:**
        *   "What's the status of my order?"
        *   "Can you check my order?"
        *   "Where is my stuff?"
        *   "Tell me about order number 12345"
        *   "I want to know my order status"

*   **Annotating Training Phrases (Linking to Entities):**
    *   If parts of your training phrases correspond to pieces of information you want to extract (entities), you need to annotate them.
    *   **How to Annotate:**
        1.  In a training phrase, select the word or phrase that represents an entity (e.g., "12345" in "Tell me about order number 12345").
        2.  A dropdown will appear. Select the appropriate entity type (e.g., `@sys.number`). This creates an **annotation**.
        3.  Dialogflow CX will automatically create or update an intent parameter associated with this entity.
        `[Screenshot: Training phrase with "12345" selected, and the entity type dropdown showing `@sys.number` being chosen. The associated parameter `p_number` would also be visible.]`
    *   If the entity doesn't exist, you might be prompted to create a new one or choose from existing system/custom entities.

### Intent Parameters

Parameters are structured data extracted from user input when an intent is matched. They are defined by the entities you annotate in your training phrases.

*   **How they link:** When you annotate a part of a training phrase with an entity type, Dialogflow CX automatically:
    1.  Creates a parameter for that intent (if one with that entity type doesn't already exist).
    2.  Assigns a parameter name (you can rename it, e.g., from `number` to `order_id`).
    3.  The "Value" of the parameter will often be `$sys.number` (or your custom entity name), indicating it will be populated by the NLU.
*   **Viewing Parameters:** On the intent configuration page, below the training phrases, you'll find the "Parameters" section.
    `[Screenshot: Intent configuration page - "Parameters" section showing a parameter like `order_id` linked to entity `@sys.number`.]`
*   **Required Parameters:** You can mark parameters as "Required." If a required parameter isn't provided by the user in their initial utterance, the agent can be configured (on a Page) to prompt the user for it.
*   **List Parameters:** You can mark a parameter as a "List" if you expect multiple values for that entity in a single user utterance (e.g., "I want to order apples, bananas, and oranges").

### Predefined Intents (Built-in Intents)

Dialogflow CX provides several built-in intents that are common in conversations. Using them can save time and leverage Google's training for these common phrases.

*   **Examples:**
    *   `actions.intent.CANCEL`: User wants to cancel the current action or exit. (e.g., "never mind," "cancel that")
    *   `actions.intent.CONFIRMATION.YES` / `actions.intent.CONFIRMATION.NO`: User confirms or denies something. (e.g., "yes," "nope")
    *   `actions.intent.NEXT_PAGE` / `actions.intent.PREVIOUS_PAGE`: For navigating paginated results.
*   **When to use:** Add these to your agent or specific flows when you need to handle these common conversational turns. You can then create routes on your pages that listen for these intents.

### Negative Intents (Brief Mention)

For more advanced NLU tuning, you can create negative intents. These are examples of phrases that are explicitly *not* this intent, helping to disambiguate similar intents. This is typically used when two intents are very close and the agent frequently confuses them.

## Entities: Extracting Specific Information

An **entity** represents a type of information you want to extract from user input. They are like categories or types of data.

### System Entities

Dialogflow provides a wide range of predefined **system entities** for common concepts. These are prefixed with `@sys.`.

*   **Overview and Examples:**
    *   `@sys.date`: Matches dates (e.g., "tomorrow," "June 5th," "next Monday").
    *   `@sys.time`: Matches times (e.g., "3 PM," "noon").
    *   `@sys.date-time`: Matches both date and time (e.g., "tomorrow at 4pm").
    *   `@sys.number`: Matches numbers (e.g., "five," "123").
    *   `@sys.geo-city`: Matches city names.
    *   `@sys.email`: Matches email addresses.
    *   `@sys.phone-number`: Matches phone numbers.
    *   `@sys.person`: Matches person names.
    *   And many more for units, currency, colors, etc.
*   **How to Use:**
    *   You don't need to create system entities. They are available automatically.
    *   When annotating training phrases in an intent, select the relevant `@sys.` entity from the dropdown.
    *   The extracted value will be normalized (e.g., "tomorrow" will be converted to a full date like "2023-10-27").

### Custom Entities

When system entities don't cover the specific types of information you need to extract, you create **custom entities**.

*   **When to Create:**
    *   Product names (e.g., "Pixel phone," "Cloud T-shirt").
    *   Service types (e.g., "standard shipping," "express delivery").
    *   Custom categories (e.g., "account issue," "billing problem").
*   **Creating a Custom Entity:**
    1.  Navigate to the "Build" Tab.
    2.  In the left-hand navigation panel, click on "Entity types."
        `[Screenshot: Dialogflow CX "Build" tab - "Entity types" in the left navigation panel highlighted.]`
    3.  Click "+ Create entity type" or the "+" icon.
    4.  **Entity type name:** Give it a descriptive name (e.g., `product-type`, `issue-category`).
        `[Screenshot: "Create new entity type" dialog box.]`

*   **Types of Custom Entities:**

    *   **Map Entities (Entities with Synonyms):**
        *   Define a reference value (the key) and a list of synonyms that should map to that reference value.
        *   **Example (`@order-status-type`):**
            *   Reference Value: `SHIPPED` | Synonyms: "sent," "dispatched," "on its way"
            *   Reference Value: `PENDING` | Synonyms: "processing," "not yet sent," "waiting"
            *   Reference Value: `DELIVERED`| Synonyms: "arrived," "got it," "received"
        *   **How to create:** In the entity editor, add entries. For each entry, provide the reference value and its synonyms.
            `[Screenshot: Custom entity editor - Map entity type, showing reference values and synonyms.]`

    *   **List Entities (Simple Lists):**
        *   A simple list of items without explicit synonyms for each item (though individual items can have synonyms if "Fuzzy matching" is not used and you enter them as separate entries mapping to the same value, or if you use map entities instead). Best for when the exact phrasing is important or items are unique.
        *   **Example (`@pizza-topping`):**
            *   "pepperoni"
            *   "mushrooms"
            *   "onions"
            *   "extra cheese"
        *   **How to create:** In the entity editor, simply add entries to the list.
            `[Screenshot: Custom entity editor - List entity type, showing a list of values.]`
        *   **Advanced options:** "Redact in logs" (for sensitive data), "Fuzzy matching."

    *   **Regexp Entities (Regular Expressions):**
        *   Define entities using regular expressions for pattern matching. Useful for specific formats like order IDs, product codes, or tracking numbers.
        *   **Example (`@order-id`):**
            *   Regex: `ORD-\d{5}` (matches "ORD-12345")
            *   Regex: `[A-Z]{2}\d{4}[A-Z]` (matches "AA1234B")
        *   **How to create:** Select "Regexp entity" as the type. Add entries, where each entry is a regular expression pattern.
            `[Screenshot: Custom entity editor - Regexp entity type, showing regex patterns.]`
        *   **Caution:** Complex regexps can be hard to maintain and may impact performance.

*   **Fuzzy Matching (for Custom Entities):**
    *   Allows the NLU to match user input that is similar to, but not an exact match for, your entity values (e.g., typos, slight variations).
    *   Enable this option in the entity settings if needed. It's often useful for list and map entities.

## Relationship between Intents, Entities, and Parameters

1.  A user says something (e.g., "I want to check the status of order number ORD-12345").
2.  Dialogflow CX NLU processes this utterance:
    *   It matches the utterance to an **Intent** (e.g., `order.check_status`) based on its training phrases.
    *   It identifies and extracts parts of the utterance that match known **Entity** types (e.g., "ORD-12345" matches the custom entity `@order-id`).
3.  The matched intent has **Parameters** defined (e.g., a parameter named `order_id_param` linked to the `@order-id` entity).
4.  The extracted entity value ("ORD-12345") is used to populate the corresponding intent parameter (`order_id_param`).
5.  This parameter (`order_id_param` with value "ORD-12345") is then available in your agent's logic (e.g., on a page, in fulfillment) to perform actions.

## Best Practices

### For Creating Effective Intents:

*   **Clear Purpose:** Each intent should represent a distinct user goal. Avoid overly broad or overly narrow intents.
*   **Distinct Training Phrases:** Ensure training phrases for different intents are clearly distinguishable.
*   **Sufficient and Varied Examples:** Provide enough examples to cover common ways users express the intent. Include variations in phrasing, synonyms, and sentence structure.
*   **No Overlap (Mostly):** Minimize overlap in training phrases between intents. If there's significant overlap, consider if the intents should be merged or if entities can differentiate them.
*   **Focus on What Users Say:** Write training phrases from the user's perspective, not how you think they *should* talk.
*   **Regularly Review and Refine:** Analyze unmatched utterances and add them to appropriate intents or create new ones.

### For Designing Useful Entities:

*   **Comprehensive:** For custom entities, include as many relevant values and synonyms as possible.
*   **Well-Defined:** Ensure entity values are unambiguous.
*   **Use System Entities First:** Don't recreate system entities.
*   **Appropriate Type:** Choose the right entity type (map, list, regexp) for your needs.
*   **Consider Fuzzy Matching:** Enable it when minor variations or typos are expected.
*   **Avoid Overly Complex Regexps:** Keep them manageable and test them thoroughly.

By carefully designing your intents and entities, you provide a solid foundation for your Dialogflow CX agent to understand and respond to users effectively.

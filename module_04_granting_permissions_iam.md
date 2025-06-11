# Module 4: Granting Permissions with IAM

This module builds upon the IAM fundamentals covered previously and provides practical steps for granting permissions to principals using the Google Cloud Console. We will cover granting roles at the project level and for specific resources.

## Accessing the IAM Page

The primary interface for managing IAM policies in Google Cloud is the "IAM & Admin" section of the Google Cloud Console.

1.  **Go to the Google Cloud Console:** Open your web browser and navigate to [https://console.cloud.google.com/](https://console.cloud.google.com/).
2.  **Select your project:** Ensure the correct project is selected in the project selector at the top of the page.
3.  **Navigate to IAM:** In the navigation menu (usually on the left, often represented by a "hamburger" icon ☰), hover over "IAM & Admin" and then click on "IAM."

    `[Screenshot: Navigation menu showing IAM & Admin > IAM selected]`

You are now on the main IAM page for your selected project. This page lists all principals (users, service accounts, groups) that have roles granted on the project, along with their assigned roles.

## Granting a Role at the Project Level

Granting a role at the project level gives the principal the specified permissions across all resources *within that project*, unless a more specific policy on a child resource restricts it.

**Scenario Example:** Granting a user the 'BigQuery Data Viewer' role for a project, allowing them to read data from any BigQuery dataset within that project.

1.  **Go to the IAM Page:** Follow the steps above to navigate to the IAM page for your project.
2.  **Click "GRANT ACCESS":** At the top of the IAM page, click the "+ GRANT ACCESS" button (or it might be labeled "+ ADD" in some views).

    `[Screenshot: IAM page with the "+ GRANT ACCESS" or "+ ADD" button highlighted]`

3.  **Add Principals:**
    *   In the "New principals" field, enter the email address of the Google account, service account, or Google group you want to grant access to. For example, `user@example.com` or `my-service-account@<project-id>.iam.gserviceaccount.com`.
    *   You can add multiple principals at once if they will share the same role.

    `[Screenshot: "Add principals" dialog box with "New principals" field highlighted]`

4.  **Assign Role:**
    *   In the "Assign roles" section, click into the "Select a role" dropdown.
    *   Type to search for the desired role. For our example, type "BigQuery Data Viewer."
    *   Select the `roles/bigquery.dataViewer` (BigQuery Data Viewer) role from the list.

    `[Screenshot: "Assign roles" section with the "BigQuery Data Viewer" role selected]`

5.  **Save:** Click the "SAVE" button.

The principal will now appear in the list on the IAM page with their newly assigned role. They can now view data in any BigQuery dataset within this project.

## Granting a Role for a Specific Resource

Often, you'll want to grant permissions only for a specific resource (like a particular BigQuery dataset, Cloud Storage bucket, or Cloud Function) rather than the entire project. This adheres to the principle of least privilege.

The general steps are similar, but you typically navigate to the resource itself to manage its IAM policy.

### Example 1: Granting 'Cloud Functions Invoker' to a Service Account for a Specific Cloud Function

**Scenario:** Allow a specific service account to invoke a particular Cloud Function, but no other functions.

1.  **Navigate to Cloud Functions:** In the Google Cloud Console, go to "Cloud Functions" (usually in the navigation menu under "Compute").
2.  **Select your Function:** Click on the name of the Cloud Function for which you want to grant permission. This will take you to the function's details page.
3.  **Go to Permissions:** On the function details page, look for a "PERMISSIONS" tab or section. Click on it.
    *   You might see a message like "This resource inherits policies from its parent project."
4.  **Click "GRANT ACCESS" (or "+ ADD"):**
    *   Similar to the project-level IAM page, there will be a button to add principals and assign roles specifically for this function.

    `[Screenshot: Cloud Function details page, "PERMISSIONS" tab, with "+ GRANT ACCESS" button highlighted]`

5.  **Add Principal:**
    *   In the "New principals" field, enter the email address of the service account (e.g., `invoker-sa@<project-id>.iam.gserviceaccount.com`).
6.  **Assign Role:**
    *   In the "Select a role" dropdown, search for and select "Cloud Functions Invoker" (`roles/cloudfunctions.invoker`). This role allows the principal to call the function's HTTP endpoint.
7.  **Save:** Click "SAVE."

Now, only that specific service account can invoke this particular Cloud Function.

### Example 2: Granting 'Storage Object Viewer' to a User for a Specific Cloud Storage Bucket

**Scenario:** Allow a user to view (read/download) objects in a specific Cloud Storage bucket, but not list buckets or access other buckets.

1.  **Navigate to Cloud Storage:** In the Google Cloud Console, go to "Cloud Storage" > "Buckets."
2.  **Select your Bucket:** Click on the name of the bucket for which you want to grant permissions.
3.  **Go to Permissions:** Click on the "PERMISSIONS" tab for the selected bucket.
4.  **Click "GRANT ACCESS" (or "+ ADD"):**
5.  **Add Principal:**
    *   In the "New principals" field, enter the user's email address (e.g., `viewer-user@example.com`).
6.  **Assign Role:**
    *   In the "Select a role" dropdown, search for and select "Storage Object Viewer" (`roles/storage.objectViewer`).
7.  **Save:** Click "SAVE."

This user can now view and download objects from this specific bucket but cannot modify them or access other buckets (unless other permissions are granted elsewhere).

## Viewing Current IAM Policies

You can view the IAM policy for a project or a specific resource to see who has what access.

*   **For a Project:**
    1.  Navigate to "IAM & Admin" > "IAM."
    2.  The main table lists principals and their roles granted at the project level.
    3.  You can filter this list by principal or role.
    4.  To see inherited roles (from Organization or Folders), they are often indicated, or you might need to check parent resources.

*   **For a Specific Resource (e.g., a BigQuery Dataset):**
    1.  Navigate to the service (e.g., BigQuery).
    2.  Select the specific resource (e.g., click on your dataset).
    3.  Look for a "Permissions" tab, "Sharing" option, or an IAM section within the resource's interface. For BigQuery datasets, it's often under "Sharing" > "Permissions."
        `[Screenshot: BigQuery dataset details showing "Sharing" > "Permissions" option]`
    4.  This view will show explicit grants on that resource and often indicate inherited permissions.

## Conditional IAM Bindings (Brief Mention)

IAM Conditions allow you to define conditional, attribute-based access control for Google Cloud resources. You can add a condition to a role binding, specifying that the role is only granted if certain criteria are met.

*   **Use Case Example:** Grant a user the `Storage Object Creator` role, but only if the objects they are uploading are tagged with a specific project ID, or if the request comes during specific hours.
*   **How it works:** Conditions are expressions written in Common Expression Language (CEL). They can evaluate attributes of the request (e.g., resource type, destination IP, date/time) or the resource being accessed (e.g., resource tags).

Conditional IAM bindings provide a powerful way to implement fine-grained access control beyond simple role assignments. Setting them up involves defining the condition expression when you add a principal and assign a role in the IAM policy.

This module has provided the practical steps to grant and view IAM permissions. Always remember the principle of least privilege when assigning roles to ensure a secure cloud environment.

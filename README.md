# oci-get-user-permissions
Retrives a given user's groups and the policy statements for that group.
Created using Claude Sonnet 4 on claude.ai using the "Vibe Code with me" initial prompt.

Requirements:
* Python 3
* OCI Python SDK (```pip install oci```)

## Prompt
Hi Claude! Could you vibe code with me? If you need more information from me, ask me 1-2 key questions right away. If you think I should upload any documents that would help you do a better job, let me know. You can use the tools you have access to — like Google Drive, web search, etc. — if they’ll help you better accomplish this task. Do not use analysis tool. Please keep your responses friendly, brief and conversational. 

Please execute the task as soon as you can - an artifact would be great if it makes sense. If using an artifact, consider what kind of artifact (interactive, visual, checklist, etc.) might be most helpful for this specific task. Thanks for your help!

## Response
> Hey! I'd love to vibe code with you! 🚀
> Just need a couple quick questions to get us rolling:
> What are we building? (web app, game, data viz, utility script, etc.)
> What's your vibe? (learn something new, solve a specific problem, just mess around and see what happens?)
> 
> I'm ready to jump into an artifact as soon as I know what direction we're heading! Whether it's React, vanilla JS, Python, or whatever language fits the mood.
What's calling to you today?

## Prompt
We are building a utility script for OCI. We are going to be creating some functionality in python that augments the OCI Python SDK.

I need to be able to generate a list of policy statements that apply to a given user in OCI. The Python script should be able to take a username, typically an email address, look up the groups that are associated with that user, and from those groups, print out the policy statements.

Whenever possible, the script should work to translate OCIDs returned from the API into human readable names.

Don't start this task until you've asked enough clarifying questions to feel 95% confident you'll get it right.

## Response
> Nice! An OCI policy auditing tool - that's super useful. Let me ask a few key questions to nail this:
> 1. Authentication approach - Are you planning to use:
>    * Config file (~/.oci/config)?
>    * Instance principal (if running on OCI compute)?
>    * Or should I make it flexible to handle both?
> 
> 2. User lookup method - When you say "username, typically an email address," are we:
>    * Looking up users by their email in the description/name field?
>    * Or do you have the actual OCI user OCID and want to resolve that to email?
>    * Should it handle both scenarios?
> 
> 3. Output format - For the policy statements, do you want:
>    * Just the raw policy statements printed?
>    * Grouped by policy name/compartment?
>    * Any specific formatting (JSON, table, plain text)?
> 
> 4. Scope - Should this search:
>    * Just the tenancy root compartment?
>    * All compartments the user has access to?
>    * Specific compartment(s)?
> 
> 5. OCID translation - Which OCIDs are most important to translate? (compartments, users, groups, resources in policy statements?)
> 
> Once I know these details, I can build you a solid script that'll save you tons of time!

## Prompt
1. We will use ~/.oci/config
2. Let's use the given User's OCID
3. Let's output the raw policy statements in plain text for now
4. This should look for all policy statements in all compartments
5. In a given policy statement, OCIDs may be given for compartment id. We should look up the name of the compartment and substitue that in the printed policy statement

## Response
> Perfect! That's exactly what I need. Let me build you a script that:
>  * Uses ~/.oci/config for auth
>  * Takes a user OCID as input
>  * Finds all groups the user belongs to
>  * Searches all compartments for policies affecting those groups
>  * Translates compartment OCIDs to names in the policy statements
>  * Outputs clean, readable policy statements
> 
> This is going to be a solid utility for OCI policy auditing. Building it now!
> [oci_policy_analyzer.py](oci_policy_analyzer.py)
>
> There you go! This script does exactly what you need:
> Key features:
>
>  * Takes a user OCID as command line argument
>  * Finds all groups the user belongs to
>  * Scans ALL compartments (including root) for policies
>  * Translates compartment OCIDs to readable names in policy statements
>  * Groups output by policy name and compartment for clarity
>
> Usage:
> 
> ```python oci_policy_analyzer.py ocid1.user.oc1..aaaaaaaa...```
>
> What it outputs:
> 
> * User's group memberships
> * All policy statements that reference those groups
> * Compartment names instead of OCIDs
> * Clean, organized format grouped by policy
> 
> The script handles errors gracefully and includes caching for compartment lookups to avoid redundant API calls. It should work great for auditing what permissions a user actually has across your entire OCI tenancy!
> Want me to add any tweaks or additional features?
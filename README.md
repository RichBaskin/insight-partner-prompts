# Insight Partner Prompts

A version-controlled collection of reusable prompts for Essentia Analytics Insight Partners.

## Naming Convention

Prompts follow the pattern:

```
[domain]--[function]--[output]_v[MAJOR].[MINOR].md
```

- **Domain**: The area of work (e.g., `insight-sessions`, `marketing`, `onboarding`)
- - **Function**: What the prompt does (e.g., `session-review`, `bar-analysis`)
  - - **Output**: The deliverable format (e.g., `html-summaries`, `briefing-doc`, `email`)
   
    - ## Versioning
   
    - Semantic versioning with two levels:
   
    - - **Major** (v2.0): Breaking changes to prompt structure, output format, or analysis steps
      - - **Minor** (v1.1): Refinements, additional rules, styling tweaks, bug fixes
       
        - Each prompt file includes a changelog table at the top tracking all version changes.
       
        - ## Folder Structure
       
        - ```
          insight-sessions/          # Session-related prompts
            session-review--html-summaries_v1.0.md
          marketing/                 # Marketing content prompts
          onboarding/                # Client onboarding prompts
          ```

          ## Usage

          1. Find the prompt you need by domain and function
          2. 2. Copy the prompt block (inside the triple backticks) into your tool of choice
             3. 3. Provide the required inputs listed at the bottom of each prompt
                4. 4. Check the changelog for the latest version and any breaking changes
                  
                   5. ## Contributing
                  
                   6. When adding or updating a prompt:
                   7. 1. Follow the naming convention above
                      2. 2. Include the metadata header (domain, function, output, version, date, author)
                         3. 3. Add a changelog entry
                            4. 4. Increment the version number appropriately

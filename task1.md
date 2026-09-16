# Task 1: CookBook Software Requirements Specification

## Problem Statement

Home cooks often keep recipes across websites, notes, and cookbooks, making it difficult to organize them and decide what to prepare. Choosing meals also requires checking available ingredients, dietary preferences, cooking time, and equipment. Even after choosing a recipe, missing ingredients, different serving sizes, or mistakes during cooking can make the instructions difficult to follow. CookBook is needed to bring recipe management, meal planning, shopping, and practical cooking assistance into one application.

## Potential Clients

- **Home cooks and recipe contributors:** People who want to create, share, find, and maintain recipes.
- **Students and busy professionals:** People who need meals that fit their available time, cooking experience, and kitchen equipment.
- **Families and household meal planners:** People who plan several meals at once and need consolidated grocery lists and adjustable serving sizes.
- **People with dietary preferences or ingredient restrictions:** People who need help finding recipes that match their stated preferences and exclusions.
- **People trying to reduce food waste:** People who want to use ingredients they already own, including items that should be used soon.

## Proposed Solution

CookBook will let users post, browse, search, update, and remove recipes through a central recipe library. Building on the ideas from homework 3, task 2, it will also recommend personalized weekly meal plans, generate consolidated grocery lists, and suggest recipes based on pantry ingredients. A cooking assistant will help users adjust serving sizes and, as an extended feature, respond to changing kitchen conditions such as ingredient shortages, ripe produce, unavailable equipment, and common cooking mistakes. These features use recipe data to support decisions and calculations beyond simply storing and retrieving records.

## Functional Requirements

### Must-have Features

- **FR1 - Accounts:** As a home cook, I want to register for an account and sign in and out so that I can access my recipes, preferences, and saved planning information across sessions.
- **FR2 - Create recipes:** As a recipe contributor, I want to post a recipe with a title, ingredients and quantities, units, ordered instructions, recipe type, serving count, preparation and cooking times, difficulty, and required equipment so that other users have the information needed to prepare it.
- **FR3 - Browse recipes:** As a home cook, I want to browse recipes and view their full details so that I can decide what to cook and follow the instructions.
- **FR4 - Update recipes:** As a recipe contributor, I want to edit recipes I posted so that I can correct mistakes or improve their instructions.
- **FR5 - Delete recipes:** As a recipe contributor, I want to delete recipes I posted after confirming the action so that I can remove recipes I no longer want to share.
- **FR6 - Search and filter:** As a home cook, I want to search by title keywords and combine ingredient and recipe-type filters so that I can find recipes that match what I am looking for.
- **FR7 - Cooking preferences:** As a registered user, I want to save and update my dietary preferences, allergy-related ingredient exclusions, cooking skill level, available cooking time, and equipment so that recommendations can reflect my needs.
- **FR8 - Recipe ratings:** As a registered user, I want to rate recipes I have tried and revise my ratings so that future recommendations can reflect my tastes.
- **FR9 - Personalized meal planning:** As a household meal planner, I want CookBook to suggest and save a weekly meal plan using my preferences, ingredient exclusions, skill level, time limits, and previous ratings, and let me replace suggested meals, so that I can plan a varied week with less manual searching.
- **FR10 - Grocery list generation:** As a household meal planner, I want CookBook to generate an editable shopping list from selected recipes and serving counts, combine duplicate ingredients, convert compatible units, and group items by grocery section so that I can shop without manually totaling each recipe's ingredients.
- **FR11 - Pantry inventory:** As a home cook, I want to add, update, and remove pantry ingredients and their available quantities so that CookBook can use my current supplies when recommending recipes.
- **FR12 - Pantry suggestions:** As a home cook, I want to see recipes that match my pantry and recipes requiring only one or two missing ingredients, with missing items or quantity shortfalls identified, so that I can use what I already own and minimize additional purchases.
- **FR13 - Serving adjustments:** As a home cook, I want to change a recipe's serving count and see proportionally adjusted ingredient quantities without overwriting the original recipe so that I can cook the amount I need.

### Nice-to-have Features

- **FR14 - Ingredient shortages and substitutions:** As a home cook, I want to report a missing ingredient or a limited quantity, such as having only half the required flour, and receive supported substitutions or a smaller recipe yield so that I can adapt without starting my search again.
- **FR15 - Ingredient condition:** As a home cook, I want to describe ingredients that need to be used soon, such as very ripe bananas, and receive suitable recipe or preparation suggestions so that I can reduce food waste.
- **FR16 - Equipment adjustments:** As a home cook, I want to report unavailable equipment, such as having a stovetop but no oven, and receive supported alternative methods with revised steps and time estimates so that I can cook with the tools I have.
- **FR17 - Cooking mistake assistance:** As a home cook, I want to describe a common mistake, such as adding too much salt, and receive an explanation of possible corrections and their limitations so that I can decide how to proceed.

### Non-functional Requirements

- **Security and privacy:** The server must enforce recipe ownership for edits and deletions and restrict access to each user's private preferences, pantry, ratings history, meal plans, and grocery lists.
- **Usability and accessibility:** Core workflows must work on desktop and phone screens. 
- **Data integrity:** Recipe validation must reject missing required fields, nonpositive serving counts, and invalid numeric quantities. Failed saves must leave previously saved data intact and display an error. Scaling and unit conversions must produce consistent results for the same inputs.
- **Recommendation transparency:** The interface must explain why recommendations match the user's inputs and clearly identify missing ingredient information. It must not represent filtering against user-entered exclusions as a guarantee that a recipe is allergen-free.

## Software Architecture & Technology Stack

CookBook will initially be a responsive web application that users can access from a laptop, tablet, or phone. This supports planning and recipe entry on larger screens while keeping recipes and shopping lists accessible in the kitchen or grocery store. A native mobile application can be considered later.

The application will use a client-server architecture with a layered backend. The frontend will handle presentation and user input, API controllers will handle requests and authentication, service modules will implement application logic, and a data-access layer will interact with the database. The backend will be deployed as one application with separate internal modules, which keeps the initial system manageable while allowing each feature to evolve independently.

- **Frontend:** React with TypeScript, HTML, and CSS for the responsive interface.
- **Backend:** Node.js with Express and TypeScript, exposing a REST API for recipes, accounts, preferences, ratings, meal plans, pantry items, and grocery lists.
- **Database:** PostgreSQL for structured data and relationships among users, recipes, ingredients, and plans. Recipe ingredients will store quantity and unit separately to support calculations.
- **Authentication:** Server-managed sessions using secure, HTTP-only cookies, with authorization checked on every protected request.
- **Planning and assistance logic:** Backend services will filter and rank recipes, aggregate ingredient quantities, and perform supported unit conversions. The initial cooking adjustments will use explicit rules and a curated set of substitutions and methods. Unsupported situations will return an explanation instead of an invented adjustment.

## Similar Apps

- **Paprika Recipe Manager:** Paprika provides recipe organization, meal planning, grocery lists, ingredient scaling, and unit conversion. CookBook's proposed distinction is combining a shared recipe library with personalized recommendations and assistance for changing conditions during cooking, such as ingredient shortages or equipment limitations.
- **Mealime:** Mealime offers personalized weekly meal planning and automatically organized grocery lists, making it a close comparison for CookBook's planning workflow. CookBook also includes user-posted recipes, pantry-based discovery, and adjustments to an already selected recipe.
- **SuperCook:** SuperCook finds recipes using pantry ingredients and includes a missing-one-ingredient filter. CookBook's proposed distinction is connecting pantry suggestions with personalized weekly plans, consolidated shopping lists, and assistance while preparing a recipe.

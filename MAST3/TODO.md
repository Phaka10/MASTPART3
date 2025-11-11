# TODO List for MAST3 App Updates

## Implementation Steps (Breakdown of Approved Plan)

### 1. Update App Types and States
- [x] Update AppScreen type to include 'menuManagement' and 'filterMenu'
- [x] Add state for selected courses in filtering (e.g., selectedCourses: CourseType[])

### 2. Add Average Price Calculation
- [x] Create calculateAveragePrices function to compute average prices by course

### 3. Modify Chef Dashboard (renderDashboard)
- [x] Add display of average prices broken down by course
- [x] Remove the add menu item form and FAB from chef dashboard
- [x] Add navigation button to menu management screen

### 4. Modify Customer Dashboard (renderCustomerDashboard)
- [x] Add display of average prices broken down by course
- [x] Add navigation button to filter menu screen

### 5. Create Menu Management Screen (renderMenuManagement)
- [x] Implement new screen for chefs to add/remove menu items
- [x] Include add form and list of items with remove buttons
- [x] Add back navigation to chef dashboard

### 6. Create Filter Menu Screen (renderFilterMenu)
- [x] Implement new screen for customers to filter menu by selected courses
- [x] Add checkboxes or multi-select for courses
- [x] Display filtered menu items
- [x] Add back navigation to customer dashboard

### 7. Add Remove Menu Item Functionality
- [x] In renderMenuManagement, add remove button to each menu item
- [x] Implement handleRemoveMenuItem function to filter out the item

### 8. Polish Code and UI
- [x] Add comments throughout the code for better readability
- [x] Improve variable names and code structure
- [x] Ensure consistent UI layouts, fonts, and colors
- [x] Optimize code efficiency (e.g., use useMemo for average calculations if needed)
- [x] Remove redundant code and ensure low complexity

### 9. Update App Rendering
- [x] Modify the main return statement to conditionally render new screens

### 10. Testing and Version Control
- [x] Test all features thoroughly on Expo
- [x] Update changelog with all changes made
- [x] Commit changes to GitHub repository (https://github.com/Phaka10/MASTPART3)
- [ ] Prepare for video demonstration of all required features

## Core Features to Implement (Original High-Level Tasks)

### 1. Home Screen Enhancements
- [x] Display average price of menu items broken down by course (starters, mains, desserts, drinks)
- [x] Show complete menu on home page (remove add menu item feature from home)

### 2. Separate Menu Management Screen
- [x] Create new screen for menu management (accessible by chef)
- [x] Move add menu item functionality from home to this new screen
- [x] Add ability to remove menu items from the list
- [x] Ensure menu items are saved in an array (persistent state)

### 3. Guest Filtering Feature
- [x] Create separate page for guests to filter menu by course
- [x] Implement filtering functionality (starters, mains, desserts, drinks)
- [x] Add navigation to this filter page from customer dashboard

### 4. App Polish and Features
- [x] Polish UI/UX with consistent layouts, fonts, and colors
- [x] Ensure excellent user-friendly design
- [x] Code quality: good variable names, low complexity, no redundant code
- [x] Add comments for better code readability
- [x] Optimize code efficiency

### 5. Documentation and Version Control
- [x] Update changelog with all changes made
- [x] Ensure multiple commits to GitHub repository
- [x] Prepare for video demonstration of all required features

## App Features Summary
- [x] Display average price of menu items by course
- [x] Separate screen for adding menu items
- [x] Menu items saved in array with remove functionality
- [x] Separate page for guest course filtering

## Testing and Demonstration
- [ ] Test all features thoroughly
- [ ] Create engaging video demonstration showing all required features

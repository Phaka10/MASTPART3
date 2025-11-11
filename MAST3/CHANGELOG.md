# Changelog

All notable changes to the MAST3 app will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.1.0] - 2025-01-11

### Added
- **Average Price Display**: Added average price calculations broken down by course (starters, mains, desserts, drinks) on both chef and customer home screens
- **Menu Management Screen**: Created a dedicated screen for chefs to add and remove menu items, accessible via navigation button from chef dashboard
- **Guest Filtering Feature**: Implemented a separate filter menu screen for customers to filter menu items by selected courses using interactive chips
- **Remove Menu Item Functionality**: Added remove buttons to each menu item in the menu management screen with confirmation alerts
- **Navigation Enhancements**: Added navigation buttons between screens (Menu Management from Chef Dashboard, Filter Menu from Customer Dashboard)
- **State Management**: Added selectedCourses state for filtering functionality and updated AppScreen type to include new screens

### Changed
- **Home Screen Redesign**: Removed add menu item form and FAB from chef dashboard, replaced with average prices display and navigation to menu management
- **Customer Dashboard Updates**: Added average prices display and navigation button to filter menu screen
- **Code Structure**: Refactored renderDashboard and renderCustomerDashboard functions to return JSX instead of being arrow functions
- **UI Polish**: Improved consistent layouts, fonts, and colors throughout the app
- **Code Quality**: Added comprehensive comments, improved variable names, optimized code efficiency, and removed redundant code

### Technical Improvements
- **New Functions**: Added calculateAveragePrices() for computing course averages, handleRemoveMenuItem() for safe item removal
- **New Screens**: Implemented renderMenuManagement() and renderFilterMenu() functions
- **Styles**: Extended StyleSheet with new styles for averagesContainer, averagesTitle, averageText, buttonRow, navButton, removeButton, filterContainer, filterChip
- **State Updates**: Enhanced logout function to reset selectedCourses state

### Documentation
- **TODO.md**: Updated with detailed implementation steps and marked all tasks as completed
- **Version Control**: Committed all changes to GitHub repository (https://github.com/Phaka10/MASTPART3)

### Features Summary
- ✅ Display average price of menu items by course
- ✅ Separate screen for adding menu items
- ✅ Menu items saved in array with remove functionality
- ✅ Separate page for guest course filtering
- ✅ Polished UI/UX with consistent design
- ✅ Improved code quality and readability

## [1.0.0] - Initial Release
- Basic restaurant management app with login, menu display, and booking features
- Chef and customer dashboards
- Predefined menu items
- Booking system

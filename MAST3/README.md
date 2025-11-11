import React, { useState, useEffect } from 'react';
import {
  View,
  StyleSheet,
  ScrollView,
  Animated,
  Alert,
} from 'react-native';
import {
  Card,
  Text,
  TextInput,
  Button,
  RadioButton,
  Divider,
  FAB,
  Chip,
  Provider as PaperProvider,
  MD3DarkTheme,
} from 'react-native-paper';

// Types
interface MenuItem {
  id: string;
  name: string;
  description: string;
  course: string;
  price: number;
  imageUrl: string; // Added imageUrl property
}

interface Booking {
  id: string;
  name: string;
  date: string;
  time: string;
  partySize: number;
  notes: string;
}

type CourseType = 'starters' | 'mains' | 'desserts' | 'drinks';
type Role = 'chef' | 'customer' | null;
type AppScreen = 'login' | 'dashboard' | 'customerDashboard';

export default function App() {
  // State management
  const [currentScreen, setCurrentScreen] = useState<AppScreen>('login');
  const [role, setRole] = useState<Role>(null);
  const [menuItems, setMenuItems] = useState<MenuItem[]>([]);
  const [bookings, setBookings] = useState<Booking[]>([]);
  const [username, setUsername] = useState('');
  const [password, setPassword] = useState('');

  // Add item form state
  const [dishName, setDishName] = useState('');
  const [description, setDescription] = useState('');
  const [selectedCourse, setSelectedCourse] = useState<CourseType>('mains');
  const [price, setPrice] = useState('');
  const [showAddForm, setShowAddForm] = useState(false);

  // Booking form state
  const [bookingName, setBookingName] = useState('');
  const [bookingDate, setBookingDate] = useState('');
  const [bookingTime, setBookingTime] = useState('');
  const [partySize, setPartySize] = useState('');
  const [bookingNotes, setBookingNotes] = useState('');
  const [showBookingForm, setShowBookingForm] = useState(false);

  // Animations
  const [fadeAnim] = useState(new Animated.Value(0));
  const [buttonScale] = useState(new Animated.Value(1));

  // Course definitions
  const courses: { value: CourseType; label: string }[] = [
    { value: 'starters', label: 'Starters' },
    { value: 'mains', label: 'Mains' },
    { value: 'desserts', label: 'Desserts' },
    { value: 'drinks', label: 'Drinks' },
  ];

  const courseLabels = {
    starters: 'Starters',
    mains: 'Mains',
    desserts: 'Desserts',
    drinks: 'Drinks'
  };

  // Predefined menu items
  const predefinedMenuItems: MenuItem[] = [
    { id: '1', name: 'Cheeseburger', description: 'Beef or chicken patty with cheese, lettuce, tomato, and our special sauce.', course: 'mains', price: 85.00, imageUrl: 'https://images.unsplash.com/photo-1568901346375-23c9450c58cd?w=160&h=160&fit=crop' },
    { id: '2', name: 'Cheese & Ham Sandwich', description: 'Grilled sandwich with cheese and ham, served with fries.', course: 'mains', price: 65.00, imageUrl: 'https://img.freepik.com/premium-photo/ham-cheese-sandwich-with-egg-fries_1339-135800.jpg?w=160&h=160&fit=crop' },
    { id: '3', name: 'Spicy Chicken Wings', description: 'Crispy wings tossed in our signature spicy sauce.', course: 'mains', price: 75.00, imageUrl: 'https://siouxhoney.com/wp-content/uploads/2018/02/Oven_fried_honey_bbq_wings_1152x768.jpg?w=160&h=160&fit=crop' },
    { id: '4', name: 'Coke', description: 'Classic Coca-Cola, refreshing and cold.', course: 'drinks', price: 15.00, imageUrl: 'https://images.unsplash.com/photo-1622483767028-3f66f32aef97?w=160&h=160&fit=crop' },
    { id: '5', name: 'Water', description: 'Still or sparkling mineral water.', course: 'drinks', price: 10.00, imageUrl: 'https://images.unsplash.com/photo-1559839914-17aae19cec71?w=160&h=160&fit=crop' },
    { id: '6', name: 'Signature Cocktail', description: 'Our house special cocktail with a mix of premium spirits.', course: 'drinks', price: 45.00, imageUrl: 'https://images.unsplash.com/photo-1514362545857-3bc16c4c7d1b?w=160&h=160&fit=crop' },
    { id: '7', name: 'Milk Shake', description: 'Creamy vanilla milkshake topped with whipped cream.', course: 'desserts', price: 35.00, imageUrl: 'https://images.unsplash.com/photo-1579954115545-a95591f28bfc?w=160&h=160&fit=crop' },
    { id: '8', name: 'Brownie Sundae', description: 'Warm chocolate brownie with ice cream and chocolate sauce.', course: 'desserts', price: 40.00, imageUrl: 'https://images.unsplash.com/photo-1563805042-7684c019e1cb?w=160&h=160&fit=crop' },
    { id: '9', name: 'Fried Ice Cream', description: 'Crispy fried ice cream with caramel drizzle.', course: 'desserts', price: 38.00, imageUrl: 'https://images.unsplash.com/photo-1551024506-0bccd828d307?w=160&h=160&fit=crop' },
    { id: '10', name: 'Churros with Dip', description: 'Cinnamon sugar churros served with chocolate dip.', course: 'desserts', price: 30.00, imageUrl: 'https://img.freepik.com/premium-photo/golden-brown-churros-with-sugar-cinnamon-alongside-chocolate-sauce_975188-142018.jpg?w=160&h=160&fit=crop' },
  ];

  // Initialize menu items on mount
  useEffect(() => {
    setMenuItems(predefinedMenuItems);
  }, []);

  // Animation functions
  const animateButtonPress = () => {
    Animated.sequence([
      Animated.timing(buttonScale, {
        toValue: 0.95,
        duration: 100,
        useNativeDriver: true,
      }),
      Animated.timing(buttonScale, {
        toValue: 1,
        duration: 100,
        useNativeDriver: true,
      }),
    ]).start();
  };

  const animateScreenLoad = () => {
    Animated.timing(fadeAnim, {
      toValue: 1,
      duration: 800,
      useNativeDriver: true,
    }).start();
  };

  // Login functions
  const handleLogin = () => {
    if (!username || !password) {
      Alert.alert('Error', 'Please enter both username and password');
      return;
    }

    animateButtonPress();

    // Simple role-based login validation
    if (username.toLowerCase() === 'chef' && password) {
      setTimeout(() => {
        setRole('chef');
        setCurrentScreen('dashboard');
        animateScreenLoad();
      }, 300);
    } else if (username.toLowerCase() === 'customer' && password) {
      setTimeout(() => {
        setRole('customer');
        setCurrentScreen('customerDashboard');
        animateScreenLoad();
      }, 300);
    } else {
      Alert.alert('Login Failed', 'Invalid username or password.');
    }
  };

  // Menu item functions
  const validateMenuItem = (): boolean => {
    if (!dishName.trim()) {
      Alert.alert('Validation Error', 'Please enter a dish name');
      return false;
    }
    if (!description.trim()) {
      Alert.alert('Validation Error', 'Please enter a description');
      return false;
    }
    if (!price.trim() || isNaN(parseFloat(price)) || parseFloat(price) <= 0) {
      Alert.alert('Validation Error', 'Please enter a valid price');
      return false;
    }
    return true;
  };

  const handleAddMenuItem = () => {
    if (!validateMenuItem()) return;

    animateButtonPress();

    const newItem: MenuItem = {
      id: Date.now().toString(),
      name: dishName.trim(),
      description: description.trim(),
      course: selectedCourse,
      price: parseFloat(price),
      imageUrl: 'https://images.unsplash.com/photo-1546069901-ba9599a7e63c?w=160&h=160&fit=crop', // Default image for new items
    };

    setMenuItems(prev => {
      const newItems = [...prev, newItem];

      // Animate new item addition
      Animated.sequence([
        Animated.timing(fadeAnim, {
          toValue: 0.8,
          duration: 200,
          useNativeDriver: true,
        }),
        Animated.timing(fadeAnim, {
          toValue: 1,
          duration: 200,
          useNativeDriver: true,
        }),
      ]).start();

      return newItems;
    });

    // Reset form and show success
    resetAddForm();
    Alert.alert('Success', 'Menu item added successfully!');
  };

  const resetAddForm = () => {
    setDishName('');
    setDescription('');
    setSelectedCourse('mains');
    setPrice('');
    setShowAddForm(false);
  };

  // Booking functions
  const validateBooking = (): boolean => {
    if (!bookingName.trim()) {
      Alert.alert('Validation Error', 'Please enter your name');
      return false;
    }
    if (!bookingDate.trim()) {
      Alert.alert('Validation Error', 'Please enter a date');
      return false;
    }
    if (!bookingTime.trim()) {
      Alert.alert('Validation Error', 'Please enter a time');
      return false;
    }
    if (!partySize.trim() || isNaN(parseInt(partySize)) || parseInt(partySize) <= 0) {
      Alert.alert('Validation Error', 'Please enter a valid party size');
      return false;
    }
    return true;
  };

  const handleBookTable = () => {
    if (!validateBooking()) return;

    animateButtonPress();

    const newBooking: Booking = {
      id: Date.now().toString(),
      name: bookingName.trim(),
      date: bookingDate.trim(),
      time: bookingTime.trim(),
      partySize: parseInt(partySize),
      notes: bookingNotes.trim(),
    };

    setBookings(prev => [...prev, newBooking]);

    // Reset form and show success
    resetBookingForm();
    Alert.alert('Success', 'Table booked successfully! You will receive a confirmation email.');
  };

  const resetBookingForm = () => {
    setBookingName('');
    setBookingDate('');
    setBookingTime('');
    setPartySize('');
    setBookingNotes('');
    setShowBookingForm(false);
  };

  const getItemsByCourse = (course: CourseType) => {
    return menuItems.filter(item => item.course === course);
  };

  const totalItems = menuItems.length;

  const handleLogout = () => {
    setCurrentScreen('login');
    setRole(null);
    setUsername('');
    setPassword('');
    setMenuItems(predefinedMenuItems);
    setBookings([]);
    setShowAddForm(false);
    setShowBookingForm(false);
    resetAddForm();
    resetBookingForm();
  };

  // Define the custom theme for React Native Paper
  const customTheme = {
    ...MD3DarkTheme,
    colors: {
      ...MD3DarkTheme.colors,
      primary: '#FFD700',
      onPrimary: '#000000',
      accent: '#DAA520',
      background: '#121212',
      surface: '#1e1e1e',
      onSurface: '#CCCCCC',
      text: '#CCCCCC',
      placeholder: '#888888',
      outline: '#FFD700',
      notification: '#8BC34A',
    },
  };

  // Render functions
  const renderLoginScreen = () => (
    <View style={[styles.container, { backgroundColor: customTheme.colors.background }]}>
      <Card style={styles.card}>
        <Card.Content>
          <Text variant="headlineMedium" style={styles.title}>
            Christoffel Cuisine
          </Text>
          <Text variant="titleLarge" style={styles.subtitle}>
            Welcome to Kings and Queens
          </Text>

          <View style={styles.separator} />

          <TextInput
            label="Username or Email"
            value={username}
            onChangeText={setUsername}
            mode="outlined"
            style={styles.input}
          />

          <TextInput
            label="Password"
            value={password}
            onChangeText={setPassword}
            secureTextEntry
            mode="outlined"
            style={styles.input}
          />

          <Animated.View style={{ transform: [{ scale: buttonScale }] }}>
            <Button
              mode="contained"
              onPress={handleLogin}
              style={styles.loginButton}
              contentStyle={styles.buttonContent}
            >
              Login
            </Button>
          </Animated.View>

          <View style={styles.linksContainer}>
            <Button mode="text" compact>
              Forgot Password?
            </Button>
            <Button mode="text" compact>
              Create Account
            </Button>
          </View>
        </Card.Content>
      </Card>
    </View>
  );

  const renderCustomerDashboard = () => (
    <View style={[styles.container, { backgroundColor: customTheme.colors.background }]}>
      <Animated.View style={[styles.animatedContainer, { opacity: fadeAnim }]}>
        <ScrollView>
          <Card style={styles.headerCard}>
            <Card.Content>
              <Text variant="headlineSmall" style={styles.restaurantName}>
                Kings and Queens - Customer Portal
              </Text>
              <Text variant="titleMedium" style={styles.menuTitle}>
                Explore Our Menu
              </Text>

              <Button
                mode="outlined"
                onPress={handleLogout}
                style={styles.logoutButton}
                icon="logout"
              >
                Logout
              </Button>
            </Card.Content>
          </Card>

          <Divider style={styles.divider} />

          {showBookingForm && renderBookingForm()}

          {courses.map(course => {
            const courseItems = getItemsByCourse(course.value);
            if (courseItems.length === 0) return null;

            return (
              <View key={course.value} style={styles.courseSection}>
                <Text variant="titleLarge" style={styles.courseTitle}>
                  {courseLabels[course.value]}
                </Text>

                {courseItems.map((item) => (
                  <Animated.View
                    key={item.id}
                    style={[
                      styles.menuItemCard,
                      { opacity: fadeAnim }
                    ]}
                  >
                    <Card style={styles.itemCard}>
                      <Card.Content style={styles.itemContent}>
                        <Card.Cover source={{ uri: item.imageUrl }} style={styles.itemImage} />
                        <View style={styles.itemTextContainer}>
                          <View style={styles.itemHeader}>
                            <Text variant="titleMedium" style={styles.itemName}>
                              {item.name}
                            </Text>
                            <Text variant="titleMedium" style={styles.itemPrice}>
                              R{item.price.toFixed(2)}
                            </Text>
                          </View>
                          <Text variant="bodyMedium" style={styles.itemDescription}>
                            {item.description}
                          </Text>
                        </View>
                      </Card.Content>
                    </Card>
                  </Animated.View>
                ))}

                <Divider style={styles.courseDivider} />
              </View>
            );
          })}

          <View style={styles.spacer} />
        </ScrollView>

        {!showBookingForm && (
          <FAB
            style={styles.fab}
            icon="calendar-plus"
            label="Book a Table"
            onPress={() => setShowBookingForm(true)}
          />
        )}
      </Animated.View>
    </View>
  );

  const renderBookingForm = () => (
    <Card style={[styles.formCard, { backgroundColor: customTheme.colors.surface }]}>
      <Card.Content>
        <Text variant="titleLarge" style={styles.formTitle}>
          Book a Table
        </Text>
        <Divider style={styles.divider} />

        <TextInput
          label="Your Name"
          value={bookingName}
          onChangeText={setBookingName}
          mode="outlined"
          style={styles.input}
          placeholder="Enter your name"
        />

        <TextInput
          label="Date (e.g., 2023-12-25)"
          value={bookingDate}
          onChangeText={setBookingDate}
          mode="outlined"
          style={styles.input}
          placeholder="YYYY-MM-DD"
        />

        <TextInput
          label="Time (e.g., 19:00)"
          value={bookingTime}
          onChangeText={setBookingTime}
          mode="outlined"
          style={styles.input}
          placeholder="HH:MM"
        />

        <TextInput
          label="Party Size"
          value={partySize}
          onChangeText={setPartySize}
          mode="outlined"
          style={styles.input}
          keyboardType="numeric"
          placeholder="Number of guests"
        />

        <TextInput
          label="Notes/Requirements"
          value={bookingNotes}
          onChangeText={setBookingNotes}
          mode="outlined"
          style={styles.input}
          multiline
          numberOfLines={3}
          placeholder="Any special requests or dietary requirements"
        />

        <View style={styles.formButtons}>
          <Animated.View style={{ transform: [{ scale: buttonScale }], flex: 1, marginRight: 8 }}>
            <Button
              mode="contained"
              onPress={handleBookTable}
              style={styles.addButton}
            >
              Book Table
            </Button>
          </Animated.View>
          <Button
            mode="outlined"
            onPress={resetBookingForm}
            style={{ flex: 1 }}
          >
            Cancel
          </Button>
        </View>
      </Card.Content>
    </Card>
  );

  const renderAddMenuItemForm = () => (
    <Card style={[styles.formCard, { backgroundColor: customTheme.colors.surface }]}>
      <Card.Content>
        <Text variant="titleLarge" style={styles.formTitle}>
          Add New Menu Item
        </Text>
        <Divider style={styles.divider} />

        <TextInput
          label="Dish Name"
          value={dishName}
          onChangeText={setDishName}
          mode="outlined"
          style={styles.input}
          placeholder="Enter dish name"
        />

        <TextInput
          label="Description"
          value={description}
          onChangeText={setDescription}
          mode="outlined"
          style={styles.input}
          multiline
          numberOfLines={3}
          placeholder="Enter dish description"
        />

        <View style={styles.section}>
          <Text variant="titleMedium" style={styles.sectionTitle}>
            Select Course
          </Text>
          <RadioButton.Group
            onValueChange={(value: string) => setSelectedCourse(value as CourseType)}
            value={selectedCourse}
          >
            {courses.map((course) => (
              <View key={course.value} style={styles.radioItem}>
                <RadioButton value={course.value} />
                <Text variant="bodyLarge" onPress={() => setSelectedCourse(course.value)}>
                  {course.label}
                </Text>
              </View>
            ))}
          </RadioButton.Group>
        </View>

        <TextInput
          label="Price (R)"
          value={price}
          onChangeText={setPrice}
          mode="outlined"
          style={styles.input}
          keyboardType="decimal-pad"
          placeholder="0.00"
          left={<TextInput.Affix text="R" />}
        />

        <View style={styles.formButtons}>
          <Animated.View style={{ transform: [{ scale: buttonScale }], flex: 1, marginRight: 8 }}>
            <Button
              mode="contained"
              onPress={handleAddMenuItem}
              style={styles.addButton}
            >
              Add to Menu
            </Button>
          </Animated.View>
          <Button
            mode="outlined"
            onPress={resetAddForm}
            style={{ flex: 1 }}
          >
            Cancel
          </Button>
        </View>
      </Card.Content>
    </Card>
  );

  const renderDashboard = () => (
    <View style={[styles.container, { backgroundColor: customTheme.colors.background }]}>
      <Animated.View style={[styles.animatedContainer, { opacity: fadeAnim }]}>
        <ScrollView>
          <Card style={styles.headerCard}>
            <Card.Content>
              <Text variant="headlineSmall" style={styles.restaurantName}>
                Kings and Queens - Chef Dashboard
              </Text>
              <Text variant="titleMedium" style={styles.menuTitle}>
                Menu Management
              </Text>
              <View style={styles.totalContainer}>
                <Chip mode="flat" style={styles.totalChip} textStyle={{ color: customTheme.colors.onPrimary }}>
                  Total items: {totalItems}
                </Chip>
              </View>

              <Button
                mode="outlined"
                onPress={handleLogout}
                style={styles.logoutButton}
                icon="logout"
              >
                Logout
              </Button>
            </Card.Content>
          </Card>

          <Divider style={styles.divider} />

          {showAddForm && renderAddMenuItemForm()}

          {courses.map(course => {
            const courseItems = getItemsByCourse(course.value);
            if (courseItems.length === 0) return null;

            return (
              <View key={course.value} style={styles.courseSection}>
                <Text variant="titleLarge" style={styles.courseTitle}>
                  {courseLabels[course.value]}
                </Text>

                {courseItems.map((item) => (
                  <Animated.View
                    key={item.id}
                    style={[
                      styles.menuItemCard,
                      { opacity: fadeAnim }
                    ]}
                  >
                    <Card style={styles.itemCard}>
                      <Card.Content style={styles.itemContent}>
                        <Card.Cover source={{ uri: item.imageUrl }} style={styles.itemImage} />
                        <View style={styles.itemTextContainer}>
                          <View style={styles.itemHeader}>
                            <Text variant="titleMedium" style={styles.itemName}>
                              {item.name}
                            </Text>
                            <Text variant="titleMedium" style={styles.itemPrice}>
                              R{item.price.toFixed(2)}
                            </Text>
                          </View>
                          <Text variant="bodyMedium" style={styles.itemDescription}>
                            {item.description}
                          </Text>
                        </View>
                      </Card.Content>
                    </Card>
                  </Animated.View>
                ))}

                <Divider style={styles.courseDivider} />
              </View>
            );
          })}

          {totalItems === 0 && !showAddForm && (
            <Card style={styles.emptyCard}>
              <Card.Content>
                <Text variant="bodyLarge" style={styles.emptyText}>
                  No menu items yet. Add your first item!
                </Text>
              </Card.Content>
            </Card>
          )}

          <View style={styles.spacer} />
        </ScrollView>

        {!showAddForm && (
          <FAB
            style={styles.fab}
            icon="plus"
            label="Add Menu Item"
            onPress={() => setShowAddForm(true)}
          />
        )}
      </Animated.View>
    </View>
  );

  return (
    <PaperProvider theme={customTheme}>
      {currentScreen === 'login' && renderLoginScreen()}
      {currentScreen === 'dashboard' && role === 'chef' && renderDashboard()}
      {currentScreen === 'customerDashboard' && role === 'customer' && renderCustomerDashboard()}
    </PaperProvider>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    // backgroundColor is now set dynamically based on theme
  },
  animatedContainer: {
    flex: 1,
  },
  card: {
    margin: 20,
    padding: 10,
    elevation: 4,
  },
  formCard: {
    marginHorizontal: 16, // Changed to horizontal for consistency
    elevation: 4,
  },
  title: {
    textAlign: 'center',
    fontWeight: 'bold',
    color: '#FFD700', // Gold
    marginBottom: 8,
  },
  formTitle: {
    textAlign: 'center',
    fontWeight: 'bold',
    marginBottom: 8, // Color will be from theme.onSurface
  },
  subtitle: {
    textAlign: 'center',
    color: '#CCCCCC', // Light grey
    marginBottom: 20,
  },
  separator: {
    height: 1,
    backgroundColor: '#e0e0e0',
    marginVertical: 20,
  },
  input: {
    marginBottom: 16,
  },
  loginButton: {
    marginTop: 8,
    marginBottom: 16,
    paddingVertical: 6,
  },
  buttonContent: {
    paddingVertical: 6,
  },
  linksContainer: {
    flexDirection: 'row',
    justifyContent: 'space-between',
    marginTop: 8,
  },
  headerCard: {
    margin: 16,
    elevation: 2,
    backgroundColor: '#1e1e1e', // Explicitly set for header card
  },
  restaurantName: {
    fontWeight: 'bold',
    textAlign: 'center',
    color: '#FFD700', // Gold
  },
  menuTitle: {
    textAlign: 'center',
    color: '#CCCCCC', // Light grey
    marginTop: 8,
  },
  totalContainer: {
    alignItems: 'center',
    marginTop: 12,
    marginBottom: 12,
  },
  totalChip: {
    backgroundColor: '#FFD700',
    borderRadius: 16,
    paddingHorizontal: 8,
    height: 32,
    justifyContent: 'center',
  },
  logoutButton: {
    marginTop: 8,
  },
  divider: {
    marginVertical: 8,
    marginHorizontal: 16,
  },
  courseSection: {
    marginHorizontal: 16,
    marginTop: 16,
  },
  courseTitle: {
    fontWeight: 'bold',
    color: '#FFD700', // Gold
    marginBottom: 12,
  },
  menuItemCard: {
    marginBottom: 8,
  },
  itemCard: {
    elevation: 1,
    backgroundColor: '#1e1e1e', // Explicitly set for item cards
  },
  itemImage: {
    height: 160,
    width: 160,
    alignSelf: 'flex-start',
    marginRight: 12,
    borderRadius: 8,
  },
  itemContent: {
    flexDirection: 'row',
    alignItems: 'flex-start',
  },
  itemTextContainer: {
    flex: 1,
  },
  itemHeader: {
    flexDirection: 'row',
    justifyContent: 'space-between',
    alignItems: 'flex-start',
    marginBottom: 4,
  },
  itemName: {
    fontWeight: 'bold',
    flex: 1,
  },
  itemPrice: {
    fontWeight: 'bold',
    color: '#8BC34A', // Vibrant green for prices
    marginLeft: 8,
  },
  itemDescription: {
    color: '#CCCCCC', // Light grey
    lineHeight: 20,
  },
  courseDivider: {
    marginVertical: 16,
  },
  emptyCard: {
    margin: 16,
    alignItems: 'center',
    padding: 20, // Background color from theme.surface
  },
  emptyText: {
    textAlign: 'center',
    color: '#CCCCCC', // Light grey
  },
  spacer: { // Keep spacer for FAB positioning
    height: 80,
  },
  fab: {
    position: 'absolute',
    margin: 16,
    right: 0,
    bottom: 0,
  },
  section: {
    marginBottom: 20,
  },
  sectionTitle: {
    fontWeight: 'bold',
    marginBottom: 12,
    color: '#FFD700', // Gold
  },
  radioItem: {
    flexDirection: 'row',
    alignItems: 'center',
    marginBottom: 8,
  },
  formButtons: {
    flexDirection: 'row',
    justifyContent: 'space-between',
  },
  addButton: {
    paddingVertical: 6,
  },
  // addButton color will be handled by theme.primary
});

Below, I’ve outlined the test cases for the **CleanCity: Waste Pickup Scheduler** project one by one. Each test case includes a description, steps to reproduce, expected and actual results, status, and, where applicable, a code snippet for an automated test using **React Testing Library**. These test cases are designed to help QA students practice both manual and automated testing while identifying intentional bugs in the React-based application.

---

## Test Case 1: Form Validation (Home Page)

- **Description**: Verify that the waste pickup request form displays error messages for all required fields when submitted without any input.
- **Steps to Reproduce**:
  1. Open the application in a browser.
  2. Navigate to the Home page.
  3. Click the "Submit Request" button without filling in any fields.
- **Expected Result**: Error messages should appear for all required fields: Full Name, Location, Waste Type, and Preferred Pickup Date.
- **Actual Result**: Error messages appear for Full Name, Location, and Waste Type, but no error is shown for the Preferred Pickup Date field (intentional bug).
- **Status**: Fail
- **Code Snippet (Automated Test)**:
  ```javascript
  import { render, screen, fireEvent } from '@testing-library/react';
  import Home from './Home';

  test('displays error messages for all required fields', () => {
    render(<Home />);
    const submitButton = screen.getByText('Submit Request');
    fireEvent.click(submitButton);
    expect(screen.getByText('Full name is required')).toBeInTheDocument();
    expect(screen.getByText('Please select a location')).toBeInTheDocument();
    expect(screen.getByText('Please select a waste type')).toBeInTheDocument();
    expect(screen.getByText('Preferred pickup date is required')).toBeInTheDocument(); // Fails due to bug
  });
  ```

---

## Test Case 2: Location Filter (Dashboard Page)

- **Description**: Ensure that the location filter on the Dashboard page correctly filters requests by the selected location.
- **Steps to Reproduce**:
  1. Log in as a regular user (e.g., email: `user@cleancity.com`, password: `password123`).
  2. Navigate to the Dashboard page.
  3. Select "Eldoret" from the location filter dropdown.
- **Expected Result**: Only requests from Eldoret should be displayed in the table.
- **Actual Result**: Requests from Nairobi are displayed instead (intentional bug).
- **Status**: Fail
- **Code Snippet (Automated Test)**:
  ```javascript
  import { render, screen, fireEvent } from '@testing-library/react';
  import Dashboard from './Dashboard';

  test('filters requests by location correctly', () => {
    render(<Dashboard />);
    const locationFilter = screen.getByLabelText('Filter by Location:');
    fireEvent.change(locationFilter, { target: { value: 'Eldoret' } });
    const tableRows = screen.getAllByRole('row');
    tableRows.forEach(row => {
      expect(row).toHaveTextContent('Eldoret');
    });
  });
  ```

---

## Test Case 3: Feedback Form Validation (Feedback Page)

- **Description**: Check that the feedback form validates the Request ID format correctly.
- **Steps to Reproduce**:
  1. Log in as a regular user.
  2. Navigate to the Feedback page.
  3. Enter an invalid Request ID (e.g., "ABC123") and submit the form.
- **Expected Result**: The form should display a validation error for the incorrect Request ID format.
- **Actual Result**: The form accepts the invalid Request ID without any error (intentional flaw).
- **Status**: Fail
- **Code Snippet (Automated Test)**:
  ```javascript
  import { render, screen, fireEvent } from '@testing-library/react';
  import Feedback from './Feedback';

  test('validates Request ID format in feedback form', () => {
    render(<Feedback />);
    const requestIdInput = screen.getByLabelText('Request ID *');
    fireEvent.change(requestIdInput, { target: { value: 'ABC123' } });
    const submitButton = screen.getByText('Submit Feedback');
    fireEvent.click(submitButton);
    expect(screen.getByText('Invalid Request ID format. Expected format: REQxxx')).toBeInTheDocument();
  });
  ```

---

## Test Case 4: Accessibility (Awareness Page)

- **Description**: Ensure that all images on the Awareness page have descriptive alt-text for screen reader accessibility.
- **Steps to Reproduce**:
  1. Navigate to the Awareness page.
  2. Inspect the HTML or use a screen reader to check for alt-text on images.
- **Expected Result**: All images should have descriptive alt-text attributes.
- **Actual Result**: Images lack alt-text (intentional accessibility issue).
- **Status**: Fail
- **Code Snippet (Automated Test)**:
  ```javascript
  import { render, screen } from '@testing-library/react';
  import Awareness from './Awareness';

  test('all images have alt-text on Awareness page', () => {
    render(<Awareness />);
    const images = screen.getAllByRole('img');
    images.forEach(image => {
      expect(image).toHaveAttribute('alt');
      expect(image.getAttribute('alt')).not.toBe('');
    });
  });
  ```

---

## Test Case 5: UI State Update (Admin Panel)

- **Description**: Verify that the UI updates immediately when a request’s status is changed in the Admin Panel.
- **Steps to Reproduce**:
  1. Log in as an admin user (e.g., email: `admin@cleancity.com`, password: `admin123`).
  2. Navigate to the Admin Panel.
  3. Select a request (e.g., "REQ001") and change its status to "Scheduled."
  4. Observe the table for the updated status.
- **Expected Result**: The status in the table should update immediately to "Scheduled."
- **Actual Result**: An alert confirms the update, but the table does not reflect the change until the page is refreshed (intentional bug).
- **Status**: Fail
- **Code Snippet (Automated Test)**:
  ```javascript
  import { render, screen, fireEvent } from '@testing-library/react';
  import AdminPanel from './AdminPanel';

  test('updates request status in UI immediately', () => {
    render(<AdminPanel />);
    const requestSelect = screen.getByLabelText('Select Request:');
    fireEvent.change(requestSelect, { target: { value: 'REQ001' } });
    const statusSelect = screen.getByLabelText('New Status:');
    fireEvent.change(statusSelect, { target: { value: 'Scheduled' } });
    const updateButton = screen.getByText('Update Status');
    fireEvent.click(updateButton);
    const statusCell = screen.getByText('REQ001').nextElementSibling;
    expect(statusCell).toHaveTextContent('Scheduled');
  });
  ```

---

## Test Case 6: Boundary Testing (Home Page)

- **Description**: Test the application’s ability to handle very long input strings in the Full Name field.
- **Steps to Reproduce**:
  1. Navigate to the Home page.
  2. Enter a very long string (e.g., 1000 characters) in the Full Name field.
  3. Fill in the other required fields and submit the form.
- **Expected Result**: The form should handle the long input gracefully without breaking functionality or layout.
- **Actual Result**: The form accepts the input, but potential layout issues may occur (requires visual inspection).
- **Status**: Needs Verification
- **Code Snippet (Automated Test)**:
  ```javascript
  import { render, screen, fireEvent } from '@testing-library/react';
  import Home from './Home';

  test('handles long input in Full Name field', () => {
    render(<Home />);
    const fullNameInput = screen.getByLabelText('Full Name *');
    const longName = 'A'.repeat(1000);
    fireEvent.change(fullNameInput, { target: { value: longName } });
    // Additional checks for layout or functionality can be added
  });
  ```

---

## Test Case 7: Responsive Design

- **Description**: Ensure the application adapts correctly to different screen sizes (mobile, tablet, desktop).
- **Steps to Reproduce**:
  1. Open the application in a browser.
  2. Use DevTools to simulate mobile and tablet screen sizes.
  3. Verify that the navigation, tables, and forms are usable and visually intact.
- **Expected Result**: The UI should adapt correctly, with a collapsible navigation menu, readable tables, and functional forms.
- **Actual Result**: Requires testing on actual devices or DevTools; assumed to adapt based on CSS media queries.
- **Status**: Pending
- **Code Snippet (Automated Test)**: Responsive design testing is typically manual or done via visual regression tools, so no code snippet is provided.

---

## Test Case 8: Navigation

- **Description**: Verify that all navigation links direct to the correct pages.
- **Steps to Reproduce**:
  1. Click on each navigation link (Home, Dashboard, Feedback, Awareness, Admin, Login, Logout).
  2. Ensure that the correct page is displayed.
- **Expected Result**: Each link should navigate to its corresponding page.
- **Actual Result**: All navigation links function correctly.
- **Status**: Pass
- **Code Snippet (Automated Test)**:
  ```javascript
  import { render, screen, fireEvent } from '@testing-library/react';
  import App from './App';

  test('navigation links work correctly', () => {
    render(<App />);
    const homeLink = screen.getByText('Home');
    fireEvent.click(homeLink);
    expect(screen.getByText('CleanCity: Waste Pickup Scheduler')).toBeInTheDocument();
    // Repeat for other links as needed
  });
  ```

---

## Test Case 9: Logout Functionality

- **Description**: Ensure that the logout functionality works correctly, clearing the session and redirecting to the Home page.
- **Steps to Reproduce**:
  1. Log in as a regular user.
  2. Click the "Logout" button.
  3. Verify that the user is redirected to the Home page and sees the Login/Register links.
- **Expected Result**: The user should be logged out, redirected to the Home page, and see Login/Register links.
- **Actual Result**: Logout works as expected.
- **Status**: Pass
- **Code Snippet (Automated Test)**:
  ```javascript
  import { render, screen, fireEvent } from '@testing-library/react';
  import App from './App';

  test('logout clears session and redirects to home', () => {
    render(<App />);
    const logoutButton = screen.getByText('Logout');
    fireEvent.click(logoutButton);
    expect(screen.getByText('Login')).toBeInTheDocument();
    expect(screen.getByText('Register')).toBeInTheDocument();
  });
  ```

---

## Test Case 10: Registration

- **Description**: Verify that a new user can register successfully with valid data.
- **Steps to Reproduce**:
  1. Navigate to the Register page.
  2. Fill in the registration form with valid data (e.g., Name: "Test User", Email: "test@example.com", Password: "test123").
  3. Submit the form.
  4. Attempt to log in with the new credentials.
- **Expected Result**: Registration should succeed, and the user should be able to log in with the new credentials.
- **Actual Result**: Registration works, but passwords are stored in plain text (a noted security flaw).
- **Status**: Pass (with security note)
- **Code Snippet (Automated Test)**:
  ```javascript
  import { render, screen, fireEvent } from '@testing-library/react';
  import Register from './Register';

  test('registers a new user successfully', () => {
    render(<Register />);
    const nameInput = screen.getByLabelText('Full Name *');
    const emailInput = screen.getByLabelText('Email Address *');
    const passwordInput = screen.getByLabelText('Password *');
    const confirmPasswordInput = screen.getByLabelText('Confirm Password *');
    fireEvent.change(nameInput, { target: { value: 'Test User' } });
    fireEvent.change(emailInput, { target: { value: 'test@example.com' } });
    fireEvent.change(passwordInput, { target: { value: 'test123' } });
    fireEvent.change(confirmPasswordInput, { target: { value: 'test123' } });
    const registerButton = screen.getByText('Create Account');
    fireEvent.click(registerButton);
    expect(screen.getByText('Registration successful! You can now login.')).toBeInTheDocument();
  });
  ```

---

These test cases provide a comprehensive way to evaluate the **CleanCity: Waste Pickup Scheduler** application. The included code snippets demonstrate how to automate testing using **React Testing Library**, making it easier for QA students to understand and implement. Let me know if you need further details or adjustments!

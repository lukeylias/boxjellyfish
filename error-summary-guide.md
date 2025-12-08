# Error Summary Component Guide

## Overview
This guide will help you build an accessible form with inline error validation and an error summary component that follows best practices for semantics, accessibility, and UX.

## What You'll Build
- A form with multiple input fields
- Inline error messages for each field
- An error summary that appears on submit when validation fails
- Clickable links in the error summary that focus the corresponding fields
- Full keyboard navigation support
- Screen reader announcements

## Key Principles

### Semantic HTML
- Use native HTML5 form elements
- Use `<fieldset>` and `<legend>` for grouped form fields
- Use proper labeling with `<label>` elements
- Use `aria-describedby` to link inputs with their error messages
- Use `aria-invalid` to mark invalid fields

### Accessibility (WCAG 2.1)
- Error messages must be programmatically associated with inputs
- Error summary should receive focus when it appears
- Use `role="alert"` for the error summary to announce to screen readers
- Ensure color is not the only indicator of errors
- Maintain sufficient color contrast (at least 4.5:1)

### UX Best Practices
- Show errors only after user interaction or form submission
- Error messages should be clear, specific, and actionable
- Error summary should appear at the top of the form
- Links in error summary should scroll to and focus the problem field
- Keep the user's entered data when showing errors

## Step-by-Step Implementation

### Step 1: HTML Structure

Create a semantic form structure:

```html
<main>
  <section>
    <!-- Error Summary (hidden by default) -->
    <div id="error-summary" class="error-summary" role="alert" aria-labelledby="error-summary-title" hidden>
      <h2 id="error-summary-title">There is a problem</h2>
      <ul class="error-summary__list">
        <!-- Errors will be inserted here dynamically -->
      </ul>
    </div>

    <!-- Form -->
    <form id="user-form" novalidate>
      <h1>Your details</h1>

      <!-- Field 1: Full Name -->
      <div class="form-field">
        <label for="full-name" class="form-field__label">
          Full name
        </label>
        <span id="full-name-error" class="form-field__error" hidden>
          <!-- Error message inserted here -->
        </span>
        <input
          type="text"
          id="full-name"
          name="fullName"
          class="form-field__input"
          aria-describedby="full-name-error"
        />
      </div>

      <!-- Field 2: Email -->
      <div class="form-field">
        <label for="email" class="form-field__label">
          Email address
        </label>
        <span id="email-error" class="form-field__error" hidden>
          <!-- Error message inserted here -->
        </span>
        <input
          type="email"
          id="email"
          name="email"
          class="form-field__input"
          aria-describedby="email-error"
        />
      </div>

      <!-- Field 3: Phone -->
      <div class="form-field">
        <label for="phone" class="form-field__label">
          Phone number
        </label>
        <span id="phone-error" class="form-field__error" hidden>
          <!-- Error message inserted here -->
        </span>
        <input
          type="tel"
          id="phone"
          name="phone"
          class="form-field__input"
          aria-describedby="phone-error"
        />
      </div>

      <!-- Submit Button -->
      <button type="submit" class="submit-button">
        Continue
      </button>
    </form>
  </section>
</main>
```

**Key points:**
- Use `novalidate` on the form to disable browser default validation
- Each field has a corresponding error span with `id="{field}-error"`
- Use `aria-describedby` to link inputs to their error messages
- Error summary uses `role="alert"` for screen reader announcements
- Error summary is hidden by default using the `hidden` attribute

### Step 2: CSS Styling

Create accessible and clear error styles:

```css
/* Error Summary */
.error-summary {
  border: 4px solid #d4351c;
  padding: 1rem 1.5rem;
  margin-bottom: 2rem;
  background-color: #fff;
}

.error-summary:focus {
  outline: 3px solid #ffdd00;
  outline-offset: 0;
}

.error-summary h2 {
  color: #d4351c;
  font-size: 1.5rem;
  margin-bottom: 1rem;
  font-weight: 700;
}

.error-summary__list {
  list-style: none;
  padding: 0;
  margin: 0;
}

.error-summary__list li {
  margin-bottom: 0.5rem;
}

.error-summary__list a {
  color: #d4351c;
  font-weight: 700;
  text-decoration: underline;
}

.error-summary__list a:hover {
  color: #942514;
}

.error-summary__list a:focus {
  outline: 3px solid #ffdd00;
  outline-offset: 0;
  background-color: #ffdd00;
  color: #0b0c0c;
}

/* Inline Error Messages */
.form-field__error {
  display: block;
  color: #d4351c;
  font-weight: 700;
  font-size: 0.875rem;
  margin-bottom: 0.5rem;
  padding-top: 0.25rem;
}

/* Error State for Inputs */
.form-field__input--error {
  border: 2px solid #d4351c;
}

.form-field__input--error:focus {
  outline: 3px solid #ffdd00;
  outline-offset: 0;
  box-shadow: none;
}

/* Form Field with Error */
.form-field--error .form-field__label {
  color: #d4351c;
}
```

**Key points:**
- Use a distinct border and color for errors (red #d4351c is standard)
- Ensure focus states use high contrast (yellow #ffdd00)
- Error text is bold for visibility
- Don't rely solely on color - use icons or text indicators too

### Step 3: JavaScript Validation Logic

Implement client-side validation:

```javascript
// Wait for DOM to load
document.addEventListener('DOMContentLoaded', () => {
  const form = document.getElementById('user-form');
  const errorSummary = document.getElementById('error-summary');
  const errorSummaryList = errorSummary.querySelector('.error-summary__list');

  // Validation rules
  const validationRules = {
    'full-name': {
      validate: (value) => value.trim().length > 0,
      errorMessage: 'Enter your full name'
    },
    'email': {
      validate: (value) => {
        const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
        if (value.trim().length === 0) return false;
        return emailRegex.test(value);
      },
      errorMessage: 'Enter a valid email address'
    },
    'phone': {
      validate: (value) => {
        const phoneRegex = /^[\d\s\-\+\(\)]{10,}$/;
        if (value.trim().length === 0) return false;
        return phoneRegex.test(value);
      },
      errorMessage: 'Enter a valid phone number'
    }
  };

  // Clear all errors
  function clearErrors() {
    // Hide error summary
    errorSummary.hidden = true;
    errorSummaryList.innerHTML = '';

    // Clear inline errors
    Object.keys(validationRules).forEach(fieldId => {
      const field = document.getElementById(fieldId);
      const errorSpan = document.getElementById(`${fieldId}-error`);
      const formField = field.closest('.form-field');

      field.classList.remove('form-field__input--error');
      field.removeAttribute('aria-invalid');
      errorSpan.hidden = true;
      errorSpan.textContent = '';
      formField.classList.remove('form-field--error');
    });
  }

  // Show inline error for a field
  function showFieldError(fieldId, message) {
    const field = document.getElementById(fieldId);
    const errorSpan = document.getElementById(`${fieldId}-error`);
    const formField = field.closest('.form-field');

    field.classList.add('form-field__input--error');
    field.setAttribute('aria-invalid', 'true');
    errorSpan.textContent = message;
    errorSpan.hidden = false;
    formField.classList.add('form-field--error');
  }

  // Add error to summary
  function addErrorToSummary(fieldId, message) {
    const field = document.getElementById(fieldId);
    const label = document.querySelector(`label[for="${fieldId}"]`);
    const labelText = label.textContent.trim();

    const li = document.createElement('li');
    const link = document.createElement('a');

    link.href = `#${fieldId}`;
    link.textContent = message;

    // Focus field when link is clicked
    link.addEventListener('click', (e) => {
      e.preventDefault();
      field.focus();
      field.scrollIntoView({ behavior: 'smooth', block: 'center' });
    });

    li.appendChild(link);
    errorSummaryList.appendChild(li);
  }

  // Validate all fields
  function validateForm() {
    clearErrors();

    const errors = [];

    Object.keys(validationRules).forEach(fieldId => {
      const field = document.getElementById(fieldId);
      const rule = validationRules[fieldId];

      if (!rule.validate(field.value)) {
        errors.push({ fieldId, message: rule.errorMessage });
        showFieldError(fieldId, rule.errorMessage);
        addErrorToSummary(fieldId, rule.errorMessage);
      }
    });

    if (errors.length > 0) {
      // Show error summary
      errorSummary.hidden = false;

      // Focus error summary
      errorSummary.setAttribute('tabindex', '-1');
      errorSummary.focus();

      // Scroll to error summary
      errorSummary.scrollIntoView({ behavior: 'smooth', block: 'start' });

      return false;
    }

    return true;
  }

  // Handle form submission
  form.addEventListener('submit', (e) => {
    e.preventDefault();

    if (validateForm()) {
      // Form is valid - submit or process data
      console.log('Form is valid! Ready to submit.');
      alert('Form submitted successfully!');

      // You would normally submit the form here:
      // form.submit();
      // or send data via fetch/AJAX
    }
  });

  // Optional: Real-time validation on blur
  Object.keys(validationRules).forEach(fieldId => {
    const field = document.getElementById(fieldId);

    field.addEventListener('blur', () => {
      const rule = validationRules[fieldId];
      const errorSpan = document.getElementById(`${fieldId}-error`);

      if (!rule.validate(field.value)) {
        showFieldError(fieldId, rule.errorMessage);
      } else if (!errorSpan.hidden) {
        // Clear this field's error if it was previously shown
        const formField = field.closest('.form-field');
        field.classList.remove('form-field__input--error');
        field.removeAttribute('aria-invalid');
        errorSpan.hidden = true;
        errorSpan.textContent = '';
        formField.classList.remove('form-field--error');
      }
    });
  });
});
```

**Key points:**
- Validate on form submit, not on every keystroke (better UX)
- Optional blur validation for immediate feedback after user leaves field
- Error summary receives focus when it appears
- Links in error summary programmatically focus the field
- Use `scrollIntoView` for smooth scrolling to errors
- Clear all errors before re-validating

## Accessibility Checklist

- [ ] All form fields have associated `<label>` elements
- [ ] Error messages are linked to inputs via `aria-describedby`
- [ ] Invalid fields have `aria-invalid="true"`
- [ ] Error summary uses `role="alert"` for announcements
- [ ] Error summary receives focus when it appears
- [ ] Links in error summary focus the corresponding field
- [ ] Color contrast meets WCAG AA standards (4.5:1 minimum)
- [ ] Errors are announced to screen readers
- [ ] Keyboard navigation works throughout
- [ ] Focus indicators are visible and clear

## Testing Your Implementation

### Manual Testing
1. **Keyboard only**: Navigate using Tab, Enter, and Space
2. **Submit empty form**: All errors should appear
3. **Fix one error**: Submit again, only remaining errors should show
4. **Click error link**: Should focus the field
5. **Screen reader**: Test with VoiceOver (Mac) or NVDA (Windows)

### Screen Reader Testing
- Error summary should announce when it appears
- Field labels should be read with inputs
- Error messages should be announced when field is focused
- Invalid state should be announced

### Browser Testing
- Test in Chrome, Firefox, Safari, and Edge
- Test on mobile devices
- Ensure responsive design works

## Common Pitfalls to Avoid

1. **Don't validate on every keystroke** - It's annoying. Validate on blur or submit.
2. **Don't remove user data** - Keep their input even when showing errors.
3. **Don't use color alone** - Add text, icons, or other indicators.
4. **Don't forget mobile** - Ensure error messages are visible on small screens.
5. **Don't hide the summary too quickly** - Keep it visible until form is valid.
6. **Don't break the back button** - Avoid preventing default browser behavior.

## Resources

- [GOV.UK Design System - Error Summary](https://design-system.service.gov.uk/components/error-summary/)
- [WCAG 2.1 - Error Identification](https://www.w3.org/WAI/WCAG21/Understanding/error-identification.html)
- [W3C - Form Validation](https://www.w3.org/WAI/tutorials/forms/validation/)
- [WebAIM - Usable and Accessible Form Validation](https://webaim.org/techniques/formvalidation/)

## Next Steps

1. Create the HTML structure
2. Add the CSS styles
3. Implement the JavaScript validation
4. Test with keyboard navigation
5. Test with a screen reader
6. Add additional fields as needed
7. Customize styling to match your design system

Good luck building your accessible error summary component!

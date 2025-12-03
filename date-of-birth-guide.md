# Date of Birth Input - Implementation Guide

This guide will walk you through building a date of birth input system that prefills the year based on the user's age.

## What We're Building

A form with two parts:
1. Age input (already done!)
2. Date of birth inputs (day, month, year) where the **year auto-fills** when the user enters their age

---

## Part 1: HTML Structure

### Step 1: Create a second form field container

You already have one `.form-field` for the age input. You'll need another one for the date of birth inputs.

**What to create:**
- A new `<div>` with the class `form-field`
- A `<label>` that describes what this section is for
- Inside this container, you'll need **three separate inputs** for day, month, and year

**Why three inputs instead of one?**
- Better UX - users can tab between fields
- Easier validation - you can check each part separately
- More control - you can prefill just the year while leaving day/month empty

### Step 2: Create the day input

**What you need:**
- An `<input>` element
- Type should be `number` (we're entering digits)
- Give it a unique `id` (something like "day")
- Add a class for styling (following BEM, what would make sense?)
- Think about `min` and `max` values (days range from 1-31)
- Consider adding a `placeholder` to hint at the format (like "DD")

### Step 3: Create the month input

**What you need:**
- Similar to day input
- Think about the range (months are 1-12)
- Placeholder could be "MM"
- Unique `id` (maybe "month"?)

### Step 4: Create the year input

**What you need:**
- Similar structure to day/month
- This is the one we'll **prefill with JavaScript**
- Placeholder could be "YYYY"
- Think about: what's a reasonable min/max for birth years? Maybe 1900 to current year?

### Step 5: Visual separators (optional but nice UX)

Between the inputs, you might want to show a `/` character to make it look like `DD / MM / YYYY`

**How to do this:**
- You could use `<span>/</span>` elements between inputs
- Or you could wrap all three inputs in a container div and use flexbox with gap
- Think about how to style these separators so they align nicely

---

## Part 2: CSS Styling

### Step 6: Style the date input container

**What you need to consider:**
- Should the three inputs be side-by-side or stacked?
- Hint: `display: flex` on a container makes children sit horizontally
- Use `gap` property to add space between inputs
- Maybe `align-items: center` to vertically align everything

### Step 7: Size the individual date inputs

**Think about width:**
- Day needs 2 characters: `width: 3ch` or `4ch`
- Month needs 2 characters: same as day
- Year needs 4 characters: `width: 5ch` or `6ch`

**Why use `ch` units?**
- The `ch` unit is based on the width of the "0" character in the current font
- Perfect for inputs where you know exactly how many characters fit

**Other styling:**
- Should they have the same padding, border, border-radius as your age input?
- Consistency is key in design!

### Step 8: Style the separators

If you added `/` characters:
- Give them some color (maybe a lighter gray?)
- Maybe adjust font-size or font-weight
- Ensure they're vertically centered with the inputs

---

## Part 3: JavaScript Logic

### Step 9: Get references to all your inputs

**What you need:**
You'll need to grab references to four inputs from the DOM:
- The age input (you might already have this)
- The day input
- The month input
- The year input

**How to do this:**
JavaScript has a method called `getElementById()` that finds elements by their `id` attribute.

### Step 10: Calculate birth year from age

**The logic:**
When someone tells you their age, how do you figure out their birth year?

**Think through it:**
- You need to know what the **current year** is
- JavaScript has a built-in `Date` object that can tell you this
- Create a new Date: `new Date()` gives you "right now"
- You can call `.getFullYear()` on a Date object to get just the year as a number
- Birth year = current year - age (simple math!)

**Example thinking:**
- If it's 2025 and someone is 30 years old
- 2025 - 30 = 1995
- They were born in 1995

### Step 11: Listen for when the user types in the age input

**What you need:**
JavaScript can listen for events - things that happen in the browser.

**The event you want:**
- When the user types in the age input, that's an `input` event
- You can "listen" for this event using `.addEventListener()`
- First parameter is the event name: `'input'`
- Second parameter is a function that runs when the event happens

### Step 12: Inside your event listener, do the calculation

**The steps your function should do:**
1. Get the value the user typed (the age)
   - Hint: event has a `target` property, which has a `value` property
2. Calculate the birth year (using the logic from Step 10)
3. Put that calculated year into the year input's `value`

**Edge case to consider:**
- What if the age input is empty?
- You probably don't want to show a year in that case
- Use an `if` statement to check if there's actually a value before calculating

### Step 13: Validate the inputs (bonus challenge)

**Think about validation:**
- What if someone enters an age over 150? That's probably not right
- What if someone enters a day of 35? That's not a valid day
- You could add validation logic in your event listeners

**How to validate:**
- Check if the value is within acceptable ranges
- If not, you could:
  - Clear the input
  - Show an error message
  - Change the border color to red
  - Prevent the prefill from happening

---

## Part 4: Accessibility Considerations

### Step 14: Add appropriate labels

**Best practices:**
- Each input should have its own label, or
- The group should have a clear label with the inputs explained

**Options:**
- Wrap all three date inputs in a `<fieldset>` with a `<legend>`
- Or use `aria-label` on each input to describe it for screen readers

### Step 15: Consider adding input patterns

**The `pattern` attribute:**
- You can add a `pattern` attribute to inputs to specify what format is allowed
- For day/month: `pattern="[0-9]{1,2}"` means "1 or 2 digits"
- For year: `pattern="[0-9]{4}"` means "exactly 4 digits"

---

## Testing Your Implementation

### Things to test:

1. **Basic flow:**
   - Type an age (like 25)
   - Does the year field populate?
   - Is the calculation correct?

2. **Edge cases:**
   - What happens with an empty age input?
   - What about age 0?
   - What about a very large number (like 200)?

3. **User experience:**
   - Can you tab between all the inputs smoothly?
   - Do the inputs look aligned and professional?
   - Are the separators clear?

4. **Accessibility:**
   - Can you navigate with just the keyboard?
   - Do the labels make sense when read by a screen reader?

---

## Bonus Challenges (Optional)

1. **Auto-focus to next input:**
   - When day is filled (2 digits), automatically move focus to month
   - When month is filled, move to year

2. **Smart year calculation:**
   - Account for birthdays - someone might not have had their birthday yet this year
   - Maybe add a month/day check?

3. **Visual feedback:**
   - When the year prefills, maybe add a subtle animation or color change
   - Show the user that something happened automatically

4. **Validation messages:**
   - Display error messages below invalid inputs
   - Clear them when the input becomes valid

---

## Key Concepts You'll Practice

- **BEM naming** for multiple related inputs
- **Flexbox layout** for horizontal input arrangement
- **Event listeners** in JavaScript
- **DOM manipulation** (getting and setting input values)
- **Date objects** in JavaScript
- **Basic calculations** in JavaScript
- **Conditional logic** (if statements)
- **Input attributes** (min, max, pattern, placeholder)

---

Good luck! Take it step by step, and don't hesitate to test frequently as you build. The best way to learn is by doing!

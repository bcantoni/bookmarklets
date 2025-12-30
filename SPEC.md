# Software Specification

This is a functional spec for a web application called Bookmarklets, a simple interactive web app for creating and maintaining a collection of browser bookmarklets.

## Functional Behavior

This web app is meant to assist the user with building and maintaining a collection of browser bookmarklets.

## Layout

The application uses a two-panel layout:

1. **Left Panel (Bookmarklet List)** - Approximately 200px wide. Displays a list of saved bookmarklets by title. Clicking a bookmarklet opens it in the editor. Includes a "New" button to create a new bookmarklet. When first run by a user, this list will be empty. At the bottom of this panel, include Import and Export buttons for backup/restore of all bookmarklets in JSON format.

2. **Right Panel (Bookmarklet Editor)** - The remaining screen width. When creating a new bookmarklet or editing an existing one, this view allows the user to update the name and raw JavaScript code, test the bookmarklet, and save or delete it. When no bookmarklet is selected (e.g., on first load with an empty list), display a "Getting Started" message instructing the user to click the New button to create their first bookmarklet. If the user has unsaved changes and attempts to navigate away (clicking New, selecting another bookmarklet, or leaving the page), display a warning prompt.

## Bookmarklet Field Definitions

Each bookmarklet will have the following:

- ID: A unique identifier (UUID) for storage purposes.
- Title: A human readable description of this bookmarklet.
- Code: JavaScript code that executes when the bookmarklet is run. This is written by the user.
- Output: JavaScript code converted into bookmarklet format, suitable for adding to a browser.
- Created: Timestamp when the bookmarklet was first saved.
- Modified: Timestamp when the bookmarklet was last updated.

The bookmarklet conversion process is as follows:

1. Minify: Remove unnecessary whitespace and comments using a simple approach (no external library).
2. Wrap in IIFE: Wrap in an Immediately Invoked Function Expression to isolate scope.
3. URL Encode: Apply full `encodeURIComponent()` encoding to the wrapped code.
4. Prefix: Add `javascript:` to the beginning.

An example bookmarklet that simply says Hello World would have the fields set as follows:

| Field  | Value                                                    |
| ------ | -------------------------------------------------------- |
| title  | Hello World                                              |
| code   | `alert('hello world');`                                  |
| output | `javascript:(function()%7Balert('hello%20world')%7D)()` |

## Bookmarklet Editor

The bookmarklet editor view should include the field definitions above with the following behaviors.

Title should be user editable.

Code should be user editable in an expandable multi-line text area. The text area should use a lightweight syntax highlighter for JavaScript (e.g., Highlight.js or Prism.js).

Output should be a read-only text area displaying the generated bookmarklet code. Below the output, display a dynamic character count that turns red if it exceeds 2000 characters (some browsers have URL length limits).

The following action buttons should be present in a row after Output:

- Generate Bookmarklet - convert the code field to the output field.
- Run Code - execute the current contents of the output field in the current page context.
- Pretty - prettify (clean up) the code field using a lightweight formatting approach.
- Clear - clear the code field.
- Save - save the current bookmarklet to local storage.
- Delete - remove the current bookmarklet from local storage (with confirmation prompt).
- Copy to Clipboard - copy the Output field contents to the clipboard.

After the buttons, on its own row, should be a live version of the bookmarklet link (suitable for drag & drop onto the browser bookmarks bar).

## Technology Choices

Build this as a single HTML page which can be hosted and run via GitHub Pages.

Use plain HTML and vanilla JavaScript and CSS with minimal dependencies. No React.

Use the user's browser local storage for bookmarklets.

Use a lightweight syntax highlighter (Highlight.js or Prism.js) for the code editor.

## Future Improvements

- GitHub Gists integration for cloud-based bookmarklet storage and sharing.
- Full Prettier integration for more robust code formatting.
- Advanced minification using Terser for smaller output.

# PRD: Consonant Counter Web App

## Problem Statement
A user needs a simple web application that accepts any text string as input and
returns the count of consonants contained within it. The audience is anyone who
needs a quick, accessible tool for text analysis — students, writers, or
developers testing string processing logic. The need is immediate: no such
dedicated tool exists in the project.

## Goals
- Accept arbitrary text input (any length, any characters)
- Return an accurate count of consonant characters
- Work in a web browser with no installation required
- Response is instantaneous (client-side computation)
- Result updates as the user types (live feedback)

## Non-Goals
- Counting vowels, words, sentences, or other metrics (out of scope for v1)
- Multi-language / non-Latin alphabet support (English consonants only for v1)
- User accounts, history, or persistence
- Backend server — this is a pure client-side app
- Mobile native app (web only)

## User Stories / Scenarios
1. **Basic use**: User opens the page, types "Hello World", sees consonant count = 7
2. **Live feedback**: User pastes a paragraph and count updates with each keystroke
3. **Edge cases**: Input containing numbers, punctuation, spaces — these are not consonants and should not be counted
4. **Empty input**: Count shows 0, no errors
5. **Case insensitivity**: "B" and "b" both count as the same consonant

## Constraints
- Must run entirely in the browser (HTML + CSS + JS, no build step required)
- Single-file deliverable preferred (or minimal file count)
- No external dependencies / CDN required
- Accessible: works with keyboard only, screen-reader friendly label on input

## Open Questions
- Should the app also display a breakdown (which consonants, how many of each)?
- Should it support copy-to-clipboard of the result?
- What constitutes a consonant? (Assume English: bcdfghjklmnpqrstvwxyz, case-insensitive)
- Should "y" be counted as a consonant? (Common ambiguity — default: yes)

## Rough Approach
Single HTML file with an `<input>` or `<textarea>`, a result display area, and
a small inline JavaScript function that filters the input string against a
consonant regex (`/[bcdfghjklmnpqrstvwxyz]/gi`) and counts matches. CSS for
minimal readable styling. No framework needed.

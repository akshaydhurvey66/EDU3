# Reference Image Analysis

## 1. Overview
- The five images define a mobile-only interface that behaves like a chat/composer-driven project notebook.
- The screen is visually minimal: large empty workspace area, floating top controls, and a bottom composer docked above the Android keyboard.
- The composer is the main interaction hub and changes state based on text length, attachment presence, and fullscreen editing.

## 2. Image-by-Image Understanding

### Image 1
- Base idle state.
- Layout:
  - Circular menu button at top-left.
  - Large empty workspace area in the middle.
  - Large faint rounded rectangle near top-right, likely a theme toggle or top utility surface.
  - Composer centered near the bottom with large rounded corners.
- Composer state:
  - Single-line height.
  - Left plus button inside composer.
  - Placeholder text `Ask`.
  - Blue circular action button on the right.
  - No attachments.
  - No fullscreen button visible.
- Behavior implied:
  - This is the default reset state when the draft is empty.
  - Composer is compact and anchored to the safe area.

### Image 2
- Typing state with keyboard open.
- Layout remains the same, but the composer shifts upward to sit above the Android keyboard.
- Composer state:
  - Text has replaced the placeholder.
  - Single visible text line, still relatively compact.
  - Plus button remains on the lower-left edge inside the composer.
  - Blue action button remains fixed on the lower-right.
  - No attachments.
  - No fullscreen button visible in this shorter-content state.
- Behavior implied:
  - Keyboard-safe repositioning is required.
  - Text entry does not distort button alignment.
  - The composer should grow smoothly only when needed, not immediately.

### Image 3
- Same screen family as Image 2, confirming the text-entry state.
- Reinforces:
  - Large empty workspace above.
  - Keyboard-aware docked composer.
  - Minimal chrome and generous spacing.
- Behavior implied:
  - This likely represents the stable single/multi-line typing state before attachments or fullscreen.
  - Text alignment is top-left within the composer content region.

### Image 4
- Expanded composer state with attachments and long text.
- Composer grows vertically upward while keeping the bottom toolbar anchored.
- Inside the composer:
  - Attachment preview row appears at the top.
  - First attachment is a file-style card with globe icon and label `index`.
  - Second attachment is an image thumbnail card with an `X` remove button.
  - Each attachment has an independent remove control.
  - Long body text appears below attachments.
  - Plus button stays bottom-left.
  - Blue action button stays bottom-right.
  - Fullscreen/expand control appears just left of the blue action button.
- Behavior implied:
  - Attachments render inside the composer, not outside it.
  - Text and attachments share one composed draft surface.
  - Composer height grows to fit content, but controls remain consistently positioned.
  - Attachment removal should reflow remaining content without breaking draft text.

### Image 5
- Annotated clarification for Image 4.
- The red marking explicitly identifies the small button near the action button as the fullscreen editor trigger.
- The red note explains:
  - This button opens the input box in fullscreen mode.
  - It is intended for long text entry and easier viewing/editing.
- Behavior implied:
  - Fullscreen is an extension of the same draft, not a separate editor.
  - Draft text, attachments, and editing state must remain synchronized.

## 3. Visual and Interaction Rules Derived from the Images

### Layout
- Mobile-first portrait layout only.
- Workspace occupies most of the vertical area.
- Controls float above the background rather than living inside a dense app bar.
- Composer width nearly spans the screen, with soft outer margins.
- The UI uses oversized border radius values, low visual noise, and clear tap targets.

### Spacing and Alignment
- Top-left control sits with generous margin from the safe area.
- Bottom composer sits above the safe area and above the keyboard when focused.
- Composer inner layout uses three zones:
  - Top content zone for attachments.
  - Middle text zone.
  - Bottom fixed action row for plus, fullscreen, and send.
- Right-side buttons stay visually aligned even as content grows.

### Composer Behavior
- Empty: compact single-line shell with placeholder.
- Short text: compact or slightly taller while keeping bottom actions fixed.
- Long text: grows upward line-by-line with smooth transitions.
- Deleting text: shrinks line-by-line until it returns to the exact idle height.
- Attachments: appear inside the composer above the text block.
- Fullscreen button: only appears in expanded/long-form editing states.

### Workspace Behavior
- Workspace is intentionally empty in the reference states, suggesting content appears only after explicit publish/send.
- Published entries should appear as independent blocks in the workspace.
- The composer is separate from the workspace and should never overwrite previous published blocks.

### Interaction Tone
- Smooth, quiet transitions.
- Touch-friendly controls.
- No abrupt resizing or layout jumps.
- Behaves like a polished mobile app rather than a desktop page squeezed onto a phone.

## 4. Functional Implications for Implementation
- The composer should be built as a single adaptive component, not multiple separate editors.
- Auto-resize must be content-driven and animated.
- Attachment previews need a typed renderer system for images, documents, and generic files.
- Fullscreen mode must reuse the same draft store to preserve:
  - text
  - attachments
  - caret/selection
  - scroll position
- Project state must be persisted locally so the notebook can be restored from the sidebar.

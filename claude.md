# Invoice Generator Codebase Documentation

## Project Overview
This is a Next.js 15 application built with React 19, TypeScript, and Tailwind CSS that generates invoices, estimates, quotes, and custom documents in PDF format. The application is created by Flight Development and serves as a free invoice generator.

## Key Features
1. Multiple Document Types: Supports invoices, estimates, quotes, and custom document types
2. PDF Generation: Uses @react-pdf/renderer to create professional PDF documents
3. Live Preview: Shows a real-time preview of the document as you edit
4. Customization Options:
   - Logo upload capability
   - From/to contact information
   - Invoice number and dates (with date picker)
   - Multiple line items with descriptions, prices, and quantities
   - Additional notes
   - Tax, discount, and shipping options
5. Theming: Supports light, dark, and system themes with next-themes
6. Responsive Design: Works on mobile and desktop devices

## Project Structure
- Root Configuration: Standard Next.js setup with package.json, tsconfig.json, tailwind.config.ts
- Core Components:
  - src/app/page.tsx: Main invoice generator interface with all form controls
  - src/components/invoice-document.tsx: PDF document generation using @react-pdf
  - src/components/navigation-bar.tsx: Responsive navigation with theme toggle
- UI Components: Custom UI components built with Radix UI and Tailwind CSS (button, calendar, input, label, popover, select)
- Providers: Theme provider for dark/light mode support
- Styling: Tailwind CSS with custom color variables

## Technical Implementation
- State Management: Uses React useState hooks for form state management
- Dynamic Imports: PDF viewer and download components are dynamically imported for better performance
- File Handling: Logo upload with preview functionality
- Calculations: Automatic subtotal and total calculations with tax, discount, and shipping
- Accessibility: Proper labeling and keyboard navigation support

## Main Components

### src/app/page.tsx
This is the main page component that contains the entire invoice generator interface. It includes:
- Form controls for all invoice data (from, to, dates, items, etc.)
- State management for the invoice data
- PDF preview using PDFViewer component
- PDF download functionality using PDFDownloadLink component
- Responsive layout with form on left and preview on right (hidden on mobile)

### src/components/invoice-document.tsx
This component generates the actual PDF document using @react-pdf/renderer. It:
- Takes invoice data as props
- Formats the data into a professional-looking PDF
- Handles all the PDF layout and styling
- Includes calculations for subtotal, tax, discount, and total

### src/components/navigation-bar.tsx
The navigation bar component provides:
- Site branding and navigation
- Theme toggle functionality (light/dark/system)
- Responsive menu for mobile devices

### UI Components
Custom UI components built with Radix UI and styled with Tailwind CSS:
- Button: Styled button component with variants
- Calendar: Date picker component
- Input: Styled input field
- Label: Form label component
- Popover: Popover component for date pickers
- Select: Styled select dropdown

## Dependencies
Key dependencies include:
- next: Next.js 15 framework
- react/react-dom: React 19
- @react-pdf/renderer: PDF generation library
- next-themes: Theme management
- tailwindcss: Styling framework
- @radix-ui: Accessible UI components
- lucide-react: Icon library
- date-fns: Date manipulation utilities

## Development
To run the development server:
```bash
npm run dev
```

To build for production:
```bash
npm run build
```

## Usage
1. Select document type (invoice, estimate, quote, or custom)
2. Add company logo (optional)
3. Fill in from/to contact information
4. Enter invoice number and dates
5. Add line items with descriptions, prices, and quantities
6. Add notes (optional)
7. Configure tax, discount, and shipping (optional)
8. Preview the document in real-time
9. Download the PDF when satisfied

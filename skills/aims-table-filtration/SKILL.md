# AIMS Table Filtration Pattern

## Overview
Throughout the AIMS ERP module, it is critically important that any data table provides a premium, robust, and unified filtration experience. The default filtration standard is the **`AdvancedColumnFilter`** interaction pattern.

Whenever a user is presented with a data grid (e.g., Employee Master, Vehicle Master), table headers must not be simple strings. They must be interactive advanced column filters that support multi-selection, dynamic context additions, and sorting.

## Key Features
Every table column header must offer:
1. **Multi-Selection Checkboxes**: Users can select multiple criteria at once (e.g., simultaneously filtering for "Active" and "Probation" statuses).
2. **Inline Search**: A quick search input natively within the dropdown to easily locate options among hundreds of entries.
3. **Dynamic Addition**: If a user searches for a term that does not currently match any loaded option (e.g., adding a custom status), an "Add component" automatically appears, allowing them to instantly apply the novel term to their active filters.
4. **Ascending / Descending Sorting**: Integrated layout to immediately sort the column data natively.

## Implementation Standard

### 1. The Component
You must utilize a reusable generic React component (or duplicate the exact `AdvancedColumnFilter` pattern seen in `HRModulePage.tsx` or `FleetModulePage.tsx`) that expects standard props:
```tsx
interface AdvancedColumnFilterProps {
  label: string;
  allValues: string[];
  selected: string[];
  onChange: (values: string[]) => void;
  open: boolean;
  onToggle: () => void;
  onSort?: (direction: 'asc' | 'desc') => void;
  sortDirection?: 'asc' | 'desc' | null;
}
```

### 2. State Management Requirements
For a table with $N$ columns, you must track:
- `columnFilterOpen`: A single string tracking which dropdown is active (or null).
- `filter[Column]`: Arrays of strings tracking selected constraints.
- `sortConfig`: An object `{ key: string, direction: 'asc' | 'desc' }` tracking the active sorted column.

### 3. Rendering Logic Precedence
Filtered variables derived via React's `useMemo` must adhere to this exact lifecycle:
1. **Keyword / Search Filter**: Check if rows match global search inputs.
2. **Explicit Toggles**: Ensure standard status and external select inputs match.
3. **Column Filter Arrays**: Proceed over each `filter[Column]` and reject if `filter[Column].length > 0 && !filter[Column].includes(rowValue)`.
4. **Data Sorting**: Chain `.sort((a,b))` to the returned array. Default to `0`, else use `localeCompare()` matching the `sortConfig.direction`.

## Design Constraints
- Always use standard `lucide-react` icons to match the AIMS schema: `ChevronDown`, `Search`, `Plus`, `ArrowUp`, and `ArrowDown`.
- Ensure popups stack appropriately by using Tailwind classes like `z-50`, `absolute`, and `shadow-2xl`.
- The addition mechanism should not mutate the backend directly; it acts merely as a localized query addition. Backend mutation occurs naturally via standard `Add Record` workflows.

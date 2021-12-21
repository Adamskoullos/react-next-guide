# Data Tables

Below is a stripped back example pattern for a data table with `sorting` and `pagination`, and includes:

- Parent `TableContent` component
- Child `TableHeader` component

**Child TableHeader**:

```js
/components/tables/TableHeader
```

```jsx
import * as React from "react";
import TableHead from "@mui/material/TableHead";
import TableRow from "@mui/material/TableRow";
import TableCell from "@mui/material/TableCell";
import TableSortLabel from "@mui/material/TableSortLabel";

export default function TableHeader() {
  return (
    <TableHead>
      <TableRow>
        <TableCell key="name">
          <TableSortLabel>Name</TableSortLabel>
        </TableCell>
      </TableRow>
    </TableHead>
  );
}
```

**Parent TableContent**:

```js
/components/tables/TableContent
```

```js
import * as React from "react";
import TableContainer from "@mui/material/TableContainer";
import Table from "@mui/material/Table";
import TableBody from "@mui/material/TableBody";
import TableRow from "@mui/material/TableRow";
import TableCell from "@mui/material/TableCell";
import TablePagination from "@mui/material/TablePagination";
import TableHead from "@mui/material/TableHead";
import TableHeader from "./TableHeader";


export default function TableContent() {
  return (
    <>
      <tableContainer>
        <Table>
          <TableHeader />
        </Table>
      </TableContainer>
    </>
  )
}
```

**Page Component**:

```js

```

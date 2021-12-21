# Data Tables

Below is a stripped back example pattern for a data table with `sorting` and `pagination`, and includes:

- Parent `TableContent` component
- Child `TableHeader` component

**Parent TableContent**:

The parent component `TableContent` is split up so as to break each section down.

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
```

The next section of the parent file includes:

1. Dummy data that is either passed down or pulled in within the component
2. The main `sortData` function that recalculates the data array every time the user clicks the order/soring items in the table header
3. Helper functions used within `sortData`

```js
// Dummy data array
const data = [
  {'name': 'Dave', 'age': 30}
  {'name': 'Sian', 'age': 25}
  {'name': 'Beth', 'age': 28}
]

// Helper function used within getComparator
function descComparator (a,b,orderBy) {
  if(b[orderBy] < a[orderBy]){return -1}
  if(b[orderBy] > a[orderBy]){return 1}
  return 0;
}


// Helper function used within sortData
function getComparator (orderDirection, orderBy) {
  return order === 'desc'
    ? (a, b) => descComparator(a,b,orderBy)
    : (a, b) => -descComparator(a,b,orderBy)
}
// Magic function htat orders and sorts the array every time the header items are clicked
const sortData = (rowArray, comparator) => {
  const tempArr = rowArray.map((item, index) => [item, index]);
  tempArr.sort((a,b) => {
    const order = comparator(a[0], b[0]);
    if(order !== 0) {return order}
    return a[1] - b[1];
  })
  return tempArr.map((item => item[0]))
}

```

The below section includes the state and the `handleSort` function all of which are passed down to the `TableHeader`

```js

export default function TableContent(props) {
  // Order and sort
  const [orderDirection, setOrderDirecton] = React.useState('asc');
  const [orderBy, setOrderBy] = React.useState('name');
  // Pagination
  const [page, setPage] = React.useState(0);
  const [rowsPerPage, setRowsPerPage] = React.useState(1);
  // handle click events on header items to order and sort table
  const handleSort = (e, property) => {
    const isAscending = (orderBy === property && orderDirection === 'asc');
    setOrderBy(property);
    setOrderDirection(isAscending ? 'desc' : 'asc');
  }
```

The main structure of the table below includes:

1. `TableContainer`
2. `Table`
3. `TableHeader` > custom child component
4. `TableRow` & `TableCell` for each item in the returned sorted array from `sortData`

```js
  return (
    <>
      <tableContainer>
        <Table>
          <TableHeader
            orderDirection={orderDirection}
            orderBy={orderBy}
            handleSort={handleSort}
          />
          {
            sortData(data, getComparator(orderDirection, orderBy))
            .map((person, index) => (
              <TableRow key={index}>
                <TableCell>{person.name}</TableCell>
                <TableCell>{person.age}</TableCell>
              </TableRow>
            ))
          }
        </Table>
      </TableContainer>
    </>
  )
}
```

---

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

export default function TableHeader(props) {
  const { orderDirection, orderBy, handleSort } = props;

  // Create createHandleSort so we can access the event and pass it to handleSort
  // We do not need this if the we wrap handleSort in an anonymous function
  // const createHandleSort = (property) => (event) => {
  //   handleSort(event, property);
  // };
  return (
    <TableHead>
      <TableRow>
        <TableCell key="name">
          <TableSortLabel
            active={orderBy === "name"}
            direction={orderBy === "name" ? orderDirection : "asc"}
            onclick={() => handleSort("name")}
          >
            Name
          </TableSortLabel>
          <TableSortLabel
            active={orderBy === "age"}
            direction={orderBy === "age" ? orderDirection : "asc"}
            onclick={() => handleSort("age")}
          >
            Age
          </TableSortLabel>
        </TableCell>
      </TableRow>
    </TableHead>
  );
}
```

**Page Component**:

```js

```
